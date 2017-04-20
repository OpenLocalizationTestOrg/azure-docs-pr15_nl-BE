<properties
    pageTitle="Azure betrokkenheid bij de mobiele Web SDK API's | Microsoft Azure"
    description="De meest recente updates en procedures voor de Web SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Gebruik de API Azure Mobile Engagement in een webtoepassing

Dit document is een aanvulling op het document waarin u het [integreren van Mobile Engagement in een webtoepassing](mobile-engagement-web-integrate-engagement.md). Het biedt gedetailleerde informatie over het gebruik van de API Azure Mobile Engagement voor aangifte van de statistieken van uw toepassing.

De API van Mobile Engagement wordt geleverd door de `engagement.agent` object. De standaard Azure Mobile Engagement Web SDK alias is `engagement`. U kunt deze alias uit de SDK-configuratie opnieuw definiëren.

## <a name="mobile-engagement-concepts"></a>Betrokkenheid bij de mobiele concepten

De volgende onderdelen verfijnen gemeenschappelijke [begrippen Mobile Engagement](mobile-engagement-concepts.md) voor het webplatform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Als de gebruiker actief gedurende meer dan een paar seconden tussen twee activiteiten blijft, volgorde van activiteiten van de gebruiker gesplitst in twee afzonderlijke sessies. Deze enkele seconden de time-out voor sessies genoemd.

Als uw webtoepassing het einde van de activiteiten van gebruikers op zichzelf niet declareren (door het aanroepen van de `engagement.agent.endActivity` functie), de server voor Mobile Engagement sessie van de gebruiker binnen drie minuten na het sluiten van de pagina automatisch verloopt. Dit heet de time-out van de sessie.

### `Crash`

Automatische rapporten van niet-afgevangen uitzonderingen voor JavaScript worden niet standaard gemaakt. Echter kunt u fouten melden handmatig met behulp van de `sendCrash` (Zie de sectie over het rapporteren van crashes) werken.

## <a name="reporting-activities"></a>Activiteiten melden

Activiteit van de gebruiker melden bevat wanneer een gebruiker een nieuwe activiteit wordt gestart en wanneer de gebruiker de huidige activiteit eindigt.

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

    engagement.agent.startActivity("MyUserActivity");

U moet bellen `startActivity()` elke activiteit van de gebruiker tijd verandert. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-the-current-activity"></a>Gebruiker eindigt de huidige activiteit

    engagement.agent.endActivity();

U moet bellen `endActivity()` ten minste één keer wanneer de gebruiker de laatste activiteit is. Hierdoor weet de Mobile Engagement Web SDK is dat de gebruiker die momenteel niet actief is en dat de sessie van de gebruiker moet worden afgesloten na het verstrijken van de time-out voor sessies. Als u `startActivity()` voordat de time-out voor sessies is verlopen, de sessie te worden hervat.

Omdat er geen betrouwbare oproep tot wanneer het navigator-venster wordt gesloten, is het vaak moeilijk of onmogelijk om het einde van de activiteiten van gebruikers binnen een web-omgeving. Daarom is de server voor Mobile Engagement sessie van de gebruiker binnen drie minuten na het sluiten van de pagina automatisch verloopt.

## <a name="reporting-events"></a>Voor het melden van gebeurtenissen

Rapportage van gebeurtenissen betreft sessie en zelfstandige gebeurtenissen.

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties die worden uitgevoerd door een gebruiker tijdens de sessie van de gebruiker.

**Voorbeeld zonder extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Voorbeeld met extra gegevens:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot de sessiegebeurtenissen, kunnen zelfstandige gebeurtenissen plaatsvinden buiten de context van een sessie.

Gebruik voor die ``engagement.agent.sendEvent`` in plaats van ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Fouten melden

Rapportage van fouten heeft betrekking op fouten in sessie en zelfstandig.

### <a name="session-errors"></a>Sessie-fouten

Sessie-fouten worden meestal gebruikt om de fouten die van invloed op de gebruiker tijdens de sessie van de gebruiker zijn.

**Voorbeeld zonder extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Voorbeeld met extra gegevens:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Zelfstandige fouten

In tegenstelling tot een sessie-fouten kunnen zelfstandige fouten optreden buiten de context van een sessie.

Gebruik voor die `engagement.agent.sendError` in plaats van `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Taken melden

Rapportage van projecten omvat voor het melden van fouten en gebeurtenissen die zich tijdens een project voordoen en het rapporteren van crashes.

**Voorbeeld:**

Als u controleren van een AJAX-aanvraag wilt, gebruikt u het volgende:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Rapporteren, fouten tijdens een project

Fouten kunnen worden gerelateerd aan een actieve taak in plaats van de huidige sessie van de gebruiker.

**Voorbeeld:**

Als u een fout gemeld wilt als een AJAX-aanvraag mislukt:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Voor het melden van gebeurtenissen tijdens een project

Gebeurtenissen kunnen worden gerelateerd aan een actieve taak in plaats van de huidige sessie van de gebruiker, dank aan de `engagement.agent.sendJobEvent` functie.

Deze functie werkt precies zoals `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Fouten melden

Gebruik de `sendCrash` handmatig loopt de functie voor het rapport.

De `crashid` een tekenreeks waarmee het type crash is.
De `crash` argument is meestal de stacktrace van de crash als een tekenreeks.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Extra parameters

U kunt willekeurige gegevens koppelen aan een gebeurtenis, fout, activiteit of taak.

De gegevens worden de JSON-object (maar niet een matrix of een primitief type).

**Voorbeeld:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Limieten

Die extra parameters van toepassing zijn op het gebied van reguliere expressies voor sleutels, type en grootte.

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat de sleutels moeten beginnen met ten minste één letter, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="values"></a>Waarden

Waarden zijn beperkt tot een tekenreeks, getal en Boolean typen.

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot 1024 tekens per gesprek (nadat de Mobile Engagement Web SDK wordt gecodeerd in JSON).

## <a name="reporting-application-information"></a>Toepassingsgegevens

U kunt handmatig rapporteren voor het bijhouden van gegevens (of een andere toepassingsspecifieke gegevens) met behulp van de `sendAppInfo()` functie.

Houd er rekening mee dat deze informatie geleidelijk kan worden verzonden. Alleen de meest recente waarde voor een bepaalde sleutel worden opgeslagen voor een specifiek apparaat.

Als de gebeurtenis extra's, kunt u elk object JSON abstracte informatie over toepassingen. Houd er rekening mee dat arrays of onderliggende objecten worden behandeld als een platte reeks (met JSON serialisatie).

**Voorbeeld:**

Hier volgt een voorbeeld van code voor het verzenden van het geslacht en Geboortedatum:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Limieten

Limieten die voor de toepassingsinformatie gelden zijn op het gebied van reguliere expressies voor sleutels en grootte.

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

    ^[a-zA-Z][a-zA-Z_0-9]*

Dit betekent dat de sleutels moeten beginnen met ten minste één letter, gevolgd door de letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Toepassingsinformatie is beperkt tot 1024 tekens per gesprek (nadat de Mobile Engagement Web SDK wordt gecodeerd in JSON).

In het bovenstaande voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}

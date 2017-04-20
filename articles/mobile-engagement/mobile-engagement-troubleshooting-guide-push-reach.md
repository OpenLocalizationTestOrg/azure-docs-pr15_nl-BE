<properties 
   pageTitle="Azure mobiele betrokkenheid bij de Troubleshooting Guide - Push/bereiken" 
   description="Problemen met gebruiker interactie en kennisgeving in Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Problemen oplossen met Push- en problemen bereiken

Hieronder vindt u mogelijke problemen optreden bij hoe Azure Mobile Engagement informatie naar uw gebruikers verzendt.
 
## <a name="push-failures"></a>Push-fouten

### <a name="issue"></a>Probleem
- Dit gereedschap duwt werken niet (in app app of beide).

### <a name="causes"></a>Oorzaken
- Vele malen een push-fout is een aanwijzing dat Azure Mobile Engagement, bereiken of een andere geavanceerde voorziening van Azure Mobile Engagement niet goed geïntegreerd of een upgrade is vereist in de SDK voor het herstellen van een bekend probleem met een nieuw platform voor OS of apparaat.
- Alleen een push In App en gewoon een push om te bepalen of iets een probleem In App of van App App van testen.
- Test u vanuit de gebruikersinterface en de API als een stap om te zien welke aanvullende foutinformatie is beschikbaar beide plaatsen.
- Van App werken dit gereedschap duwt alleen als zowel Azure Mobile Engagement en het bereik zijn geïntegreerd in de SDK.
- Dit gereedschap duwt werkt niet als certificaten zijn niet geldig of Prod.-order versus DEV correct gebruikt (alleen iOS). (**Opmerking:** 'uit de app' push-meldingen kunnen niet worden bezorgd bij iOS, als u zowel de ontwikkeling (DEV) en (PROD) Productieversies van de toepassing op hetzelfde apparaat worden geïnstalleerd omdat het beveiligingstoken is gekoppeld aan uw certificaat ongeldig kunnen worden gemaakt door Apple. Dit probleem op te lossen DEV en Prod.-versies van de toepassing verwijderen en alleen de ene versie op uw apparaat opnieuw te installeren.)
- Van App push tellingen worden anders behandeld in verschillende platformen (iOS geeft minder informatie dan Android als native gereedschap duwt zijn uitgeschakeld op een apparaat, de API kan meer informatie geven dan de UI op push stats).
- App kunnen dit gereedschap duwt door klanten op OS niveau (iOS en Android) worden geblokkeerd.
- Van App worden dit gereedschap duwt weergegeven als uitgeschakeld in de gebruikersinterface van Azure Mobile Engagement als ze goed zijn niet geïntegreerd, maar zonder melding van de API mislukken kunnen.
- In App werken dit gereedschap duwt alleen als zowel Azure Mobile Engagement en het bereik zijn geïntegreerd in de SDK.
- Dit gereedschap duwt GCM en ADM werken alleen als Azure Mobile Engagement en de server zijn geïntegreerd in de SDK (alleen Android).
- In App en van App moeten dit gereedschap duwt afzonderlijk worden getest om te bepalen of het probleem van een Push- of bereiken.
- In de App dit gereedschap duwt, vereisen dat de app open te ontvangen.
- Dit gereedschap duwt zijn in App vaak setup door een opt-in of opt-out app info tag worden gefilterd.
- Als u een aangepaste categorie in Reach-app-meldingen worden weergegeven, moet u de juiste levensduur van de kennisgeving als volgt, anders het bericht kan niet worden gewist wanneer de gebruiker genegeerd.
- Als u een campagne zonder einddatum Start en een apparaat ontvangt de app in, maar wordt niet weergegeven dat nog, de gebruiker wordt nog steeds de melding de volgende keer dat ze zich aanmelden bij de app, zelfs als u handmatig de campagne eindigt ontvangen kennisgeving.
- Voor problemen met de Push API, bevestigen dat u echt de Push API in plaats van de API bereiken gebruiken wilt (Aangezien de API bereiken vaker gebruikt wordt) en dat u niet de "payload" en "kennisgever" parameters zijn verwarrend.
- Test uw campagne push met zowel een apparaat dat is aangesloten via Wi-Fi en 3G de netwerkverbinding wordt aangesloten als een mogelijke bron van problemen.

## <a name="push-testing"></a>Push-testen

### <a name="issue"></a>Probleem
- Dit gereedschap duwt kunnen worden verzonden naar een bepaald apparaat op basis van een apparaat-ID.

### <a name="causes"></a>Oorzaken

- Test apparaten zijn ingesteld voor elk platform verschillend, maar veroorzaakt een gebeurtenis in uw app op een testapparaat en op zoek naar uw apparaat-ID in de portal moeten werken als u de apparaat-ID voor alle platforms.
- Test-apparaten werken anders met IDFA vs. IDFV (alleen iOS).


## <a name="push-customization"></a>Push-aanpassing

### <a name="issue"></a>Probleem
- Geavanceerde push content item werkt niet (badge, ring, Trillen, afbeelding, enz.).
- Koppelingen van dit gereedschap duwt werken (van app app aan een website naar een locatie in de app) niet.
- Push-statistieken tonen aan dat een push niet als veel mensen, zoals verwacht verzonden is (te veel of te weinig).
- Push gedupliceerd en twee keer ontvangen.
- Niet registreren testapparaat voor Azure Mobile Engagement duwt (met uw eigen app Prod.-boekingsgroep of DEV).

### <a name="causes"></a>Oorzaken

- Als u wilt koppelen aan een specifieke locatie in app vereist "categorieën" (alleen Android).
- Diep gekoppelde schema's om gebruikers te leiden naar een andere locatie klikt u op een push-bericht moeten zijn gemaakt en beheerd door de toepassing en de OS-apparaat niet door Mobile Engagement rechtstreeks. (**Opmerking:** van app meldingen niet koppelen rechtstreeks naar locaties met iOS app zoals dat bij Android.)
- Servers voor externe afbeelding wilt kunnen gebruiken HTTP "GET" en "HEAD" voor grote afbeelding duwt werken (alleen Android).
- U kunt in uw code de agent Azure Mobile Engagement uitschakelen wanneer het toetsenbord wordt geopend en de code voor de agent Azure Mobile Engagement opnieuw te activeren wanneer het toetsenbord is gesloten, zodat het toetsenbord heeft geen invloed op het uiterlijk van uw bericht (alleen iOS).
- Sommige items niet werken in de simulaties van de test, maar alleen echte campagnes (badge, ring, Trillen, afbeelding, enz.).
- Geen server-side gegevens wordt geregistreerd wanneer u met de knop 'test' gereedschap duwt. Alleen gegevens vastgelegd voor echte push-campagnes.
- Om te helpen het probleem te isoleren, problemen oplossen met: testen, simuleren, en een echte campagne omdat elke enigszins verschillend werken.
- Levering getallen kan van invloed zijn hoe lang die uw 'in app' en 'alle tijd' campagnes zijn gepland om te worden uitgevoerd nadat een campagne wordt alleen geleverd aan gebruikers die "app" terwijl de campagne loopt (en gebruikers die hebben hun instellingen voor meldingen 'van toepassing' is ingesteld).
- De verschillen tussen hoe Android en iOS verwerken van app-meldingen kan moeilijk direct push statistieken vergelijken tussen de Android en iOS-versie van uw toepassing. Android biedt meer OS level meldingsgegevens dan iOS. Android rapporten als een native melding ontvangen, geklikt of in het midden van de melding wordt verwijderd, maar iOS deze informatie niet rapporteert als de melding wordt geklikt. 
- De belangrijkste reden dat "gedistribueerde" nummers anders zijn dan andere dan 'geleverd' getallen voor campagnes bereiken is dat "in de app' en ' app' meldingen geteld anders. "App" meldingen worden verwerkt door Mobile Engagement, maar 'van toepassing' meldingen worden verwerkt door het midden van de melding in het besturingssysteem van het apparaat.

## <a name="push-targeting"></a>Doelitems van het type push

### <a name="issue"></a>Probleem
- Ingebouwde gericht niet werkt zoals verwacht.
- App Info Tag gericht werkt niet zoals verwacht.
- Doelitems van het type geo-locatie werkt niet zoals verwacht.
- Taalopties werken niet zoals verwacht.

### <a name="causes"></a>Oorzaken

- Zorg ervoor dat u hebt geüpload app info codes via Azure Mobile Engagement UI of API.
- De push-snelheid of push quota op het niveau van de beperking of de doelgroep op het niveau van de campagne beperken kunt voorkomen dat een persoon ontvangt een specifieke push zelfs als zij voldoen aan de doelitems criteria. 
- Het instellen van een 'taal' is anders dan gericht op basis van de land- of landinstelling, dat ook anders is dan gericht op basis van geografische locatie die is gebaseerd op een locatie van de telefoon of GPS-locatie.
- In de 'taal' wordt het bericht naar een klant die niet hun apparaat ingesteld op een van de andere talen die u opgeeft.


## <a name="push-scheduling"></a>Push-planning

### <a name="issue"></a>Probleem
- Push planning werkt niet zoals verwacht (te vroeg of vertraagde verzonden).

### <a name="causes"></a>Oorzaken

- Tijdzones kunnen problemen met de planning, met name wanneer u de tijdzone voor de eindgebruikers.
- Geavanceerde push functies kunnen uitstellen gereedschap duwt.
- Gericht op basis van de instellingen (in plaats van App Info Tags) kunnen worden vertraagd telefoon duwt aangezien Azure Mobile Engagement kunnen gegevens uit de telefoon realtime aanvragen voordat u een push verzendt.
- Campagnes gemaakt zonder einddatum de push lokaal opslaan op het apparaat en de volgende keer dat de toepassing wordt geopend, zelfs als de campagne is handmatig beëindigd weergeven.
- Meer dan een campagne starten op hetzelfde moment kan langer duren voor het scannen van uw gebruikersgroep (Probeer slechts één campagne tegelijk met een maximum van vier, ook doel alleen voor uw actieve gebruikers zo starten dat oude gebruikers niet hoeven te worden gescand).
- Als u de optie 'Publiek negeren, push zal worden verzonden naar gebruikers via de API ' in de sectie "Campagne" van een campagne bereiken de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de API bereiken.
- Als u een aangepaste categorie in Reach-app-meldingen worden weergegeven, moet u als volgt de juiste levensduur van een melding, anders het bericht kan niet worden gewist wanneer de gebruiker genegeerd.

 

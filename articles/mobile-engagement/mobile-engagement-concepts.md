<properties
    pageTitle="Concepten van Mobile Engagement | Microsoft Azure"
    description="Azure Mobile Engagement concepten"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement concepten

Een paar algemene begrippen definieert Mobile Engagement voor alle ondersteunde platforms. In dit artikel wordt een korte beschrijving van deze concepten.

Dit artikel is een goede start als u bekend met Mobile Engagement bent. Controleer ook of de documentatie leest specifiek aan het platform dat u gebruikt, zoals het zal de concepten die in dit artikel met meer details en voorbeelden, alsmede eventuele beperkingen beschreven verfijnen.

## <a name="devices-and-users"></a>Apparaten en gebruikers
Mobile Engagement identificeert gebruikers door het genereren van een unieke id voor elk apparaat. Deze id, de apparaat-id wordt genoemd (of `deviceid`). Het wordt op zodanige wijze dat alle toepassingen uitvoeren van hetzelfde apparaat delen dezelfde apparaat-id gegenereerd.

Impliciet betekent dit dat Mobile Engagement van mening één apparaat is aan exact één gebruiker en dus gebruikers en apparaten vergelijkbare concepten zijn.

## <a name="sessions-and-activities"></a>Sessies en activiteiten
Een sessie is een gebruik van de toepassing die wordt uitgevoerd door een gebruiker, vanaf het moment dat de gebruiker wordt gestart, totdat de gebruiker reageert met.

Een activiteit is een gebruik van een bepaalde submap deel van de toepassing die wordt uitgevoerd door een gebruiker (meestal is dit een scherm, maar kan alles wat geschikt is voor de toepassing).

Een gebruiker kan slechts één activiteit uitvoeren op een tijdstip.

Een activiteit wordt aangeduid met een naam (beperkt tot 64 tekens) en kan eventueel extra gegevens insluiten (in de limiet van 1024 bytes).

Sessies worden automatisch berekend in de volgorde van de activiteiten die worden uitgevoerd door gebruikers. Een sessie wordt gestart wanneer de gebruiker zijn eerste activiteit begint en stopt als hij zijn laatste activiteit is voltooid. Dit betekent dat een sessie niet hoeft expliciet worden gestart of gestopt. In plaats daarvan worden activiteiten expliciet gestart of gestopt. Als er geen activiteit wordt gemeld, wordt geen sessie gerapporteerd.

## <a name="events"></a>Gebeurtenissen
Gebeurtenissen worden gebruikt om direct acties (zoals de knop ingedrukt of artikelen die worden gelezen door gebruikers).

Een gebeurtenis kan worden gerelateerd aan de huidige sessie, naar een actieve taak of het kan een zelfstandige gebeurtenis.

Een gebeurtenis wordt geïdentificeerd door een naam (beperkt tot 64 tekens) en kan eventueel extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="error"></a>Fout
Fouten worden gebruikt om problemen goed wordt herkend door de toepassing (zoals de acties van de gebruiker onjuist of API-aanroep mislukt).

Een fout kan worden gerelateerd aan de huidige sessie, naar een actieve taak of een zelfstandige fout kan zijn.

Een fout wordt aangeduid met een naam (beperkt tot 64 tekens) en kan eventueel extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="job"></a>Taak
Taken worden gebruikt voor acties met een duur van aangifte (zoals de duur van de API-aanroepen weer tijd van advertenties, de duur van achtergrondtaken of de duur van de acties van de gebruiker).

Een taak is niet gerelateerd aan een sessie, omdat een taak kan worden uitgevoerd op de achtergrond, zonder tussenkomst van de gebruiker.

Een taak wordt geïdentificeerd door een naam (beperkt tot 64 tekens) en kan eventueel extra gegevens insluiten (in de limiet van 1024 bytes).

## <a name="crash"></a>Crash
Crashes worden automatisch uitgegeven door de SDK Mobile Engagement vastlopen rapporteren van toepassingsfouten, waarin niet wordt herkend door de toepassing problemen eenvoudiger.

## <a name="application-information"></a>Informatie over de toepassingen
Toepassingsgegevens (of app info) worden gebruikers, dat wil zeggen, een label koppelen van gegevens aan de gebruikers van een toepassing (dit is vergelijkbaar met het web cookies, met dien verstande dat app info is opgeslagen op de server op de betrokkenheid van Azure Mobile platform).

App info kan worden geregistreerd met behulp van de API van Mobile Engagement SDK of met behulp van het platform voor betrokkenheid bij de mobiele apparaat API.

App-info is een sleutel/waarde-paar dat is gekoppeld aan een apparaat. De sleutel is de naam van de app-info (beperkt tot 64 ASCII-letters [a-zA-Z], getallen (0-9) en het onderstrepingsteken [_]). De waarde (beperkt tot 1024 tekens) is een tekenreeks, geheel getal, datum (jjjj-MM-dd) of Boole-waarde (true of false).

Een willekeurig aantal app info kan worden gekoppeld aan een apparaat, binnen de grenzen die zijn gedefinieerd door de betalingsvoorwaarden van Mobile Engagement. Voor een bepaalde sleutel, Mobile Engagement alleen houdt van de laatste waarde die is ingesteld (geen geschiedenis). Instellen of wijzigen van de waarde van een app info gedwongen Mobile betrokkenheid bij de criteria die zijn ingesteld op dit app info (indien aanwezig) wat betekent dat app info kan worden gebruikt voor het starten van dit gereedschap duwt realtime publiek opnieuw evalueren.

## <a name="extra-data"></a>Extra gegevens
Extra gegevens (of extra's) is ook willekeurige gegevens die kunnen worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Extra's op dezelfde manier worden gestructureerd aan JSON-objecten: ze zijn gemaakt van een structuur van sleutel/waarde-paren. Toetsen zijn beperkt tot 64 ASCII-letters [a-zA-Z], [0-9] cijfers en onderstrepingstekens (_)) en de totale grootte van extra's is beperkt tot 1024 tekens (eenmaal gecodeerd in JSON door de betrokkenheid van Mobile SDK).

De volledige structuur van de sleutel/waarde-paren wordt opgeslagen als een JSON-object. Niettemin, alleen het eerste niveau van sleutels/waarden rechtstreeks toegankelijk zijn voor sommige geavanceerde functies zoals segmenten opgesplitste is (bijvoorbeeld eenvoudig kunt u een segment genaamd "SciFi ventilatoren" die bestaat uit alle gebruikers ten minste 10 keer de gebeurtenis met de naam "content_viewed" te hebben gestuurd met de extra sleutel 'content_type' ingesteld op de waarde "scifi" in de laatste maand). Het is dus raadzaam verzenden alleen extra's gemaakt van eenvoudige lijsten van sleutel/waarde-paren met scalaire waarden (bijvoorbeeld tekenreeksen, datums, gehele getallen of Boolean).

## <a name="next-steps"></a>Volgende stappen

- [Universele SDK voor Windows Azure Mobile Engagement-overzicht](mobile-engagement-windows-store-sdk-overview.md)
- [Azure Mobile Engagement Windows Phone Silverlight SDK-overzicht](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS SDK voor Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
- [Android SDK voor betrokkenheid bij de mobiele Azure](mobile-engagement-android-sdk-overview.md)

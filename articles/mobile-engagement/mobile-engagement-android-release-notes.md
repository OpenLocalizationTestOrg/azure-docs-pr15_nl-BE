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
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Release-opmerkingen

## <a name="423-08102016"></a>4.2.3 (10/08/2016)

- Geen meer WIFI-vergrendeling.
- Een impasse vast bij het aanroepen van getDeviceId vóór init (bug geïntroduceerd in 4.2.0).

## <a name="422-05172016"></a>4.2.2 (17/05/2016)

- Verbeteringen in de stabiliteit.

## <a name="421-05102016"></a>4.2.1 (10-05/2016)

- Beveiliging: toegang tot lokale bestanden van webpagina weergeven uit te schakelen.
- Beveiliging: verwijderen `EngagementPreferenceActivity` klasse die verouderde en niet-beveiligde `PreferenceActivity` klasse.
- Beveiliging: reach activiteiten nu beschreven worden voor het gebruik van `exported="false"`, deze vlag kan ook worden gebruikt in eerdere versies van de SDK.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)

- De SDK is nu onder MIT licentie.
- Toestaan een aangepast apparaat-id op te geven bij het initialiseren SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Verbeteringen in de stabiliteit.

## <a name="414-01262016"></a>4.1.4 (26-01/2016)

- Verbeteringen in de stabiliteit.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)

- Verbeteringen in de stabiliteit.

## <a name="412-11252015"></a>4.1.2 (25/11/2015)

- Verbeteringen in de stabiliteit.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Verbeteringen in de stabiliteit.

## <a name="410-08252015"></a>4.1.0 (25/08/2015)

- Nieuw model met machtigingen voor Android M verwerken.
- Kan nu locatie functies instellen in runtime in plaats van `AndroidManifest.xml`.
- Een machtiging bug FIX: als u `ACCESS_FINE_LOCATION`, vervolgens `ACCESS_COARSE_LOCATION` is niet meer nodig hebt.
- Verbeteringen in de stabiliteit.

## <a name="400-07062015"></a>4.0.0 (06/07/2015)

-   Interne protocol wijzigingen analytics en push betrouwbaarder te maken.
-   Native push (GCM/ADM) wordt nu ook gebruikt voor in app-meldingen zodat moet u de referenties van de oorspronkelijke push voor elk type push campagne.
-   Grote afbeelding melding oplossen: ze weergegeven alleen 10s waren na wordt geduwd.
-   Een bug in de weergave te herstellen: de URL van de standaard actie is ook uitvoeren op een koppeling te klikken.
-   Een zeldzame crash die betrekking hebben op het beheer van lokale opslag oplossen.
-   Dynamische configuratie string beheer oplossen.
-   OVEREENKOMST bijwerken.

## <a name="300-02172015"></a>3.0.0 (17/02/2015)

-   Eerste versie van Azure mobiele Engagement
-   appId-configuratie vervangen door een tekenreeks verbindingsconfiguratie.
-   API voor het verzenden en willekeurige XMPP-berichten ontvangen van willekeurige XMPP entiteiten verwijderd.
-   API voor het verzenden en ontvangen van berichten tussen apparaten verwijderd.
-   Verbeteringen in de beveiliging.
-   Bijhouden van Google Play en SmartAd verwijderd.

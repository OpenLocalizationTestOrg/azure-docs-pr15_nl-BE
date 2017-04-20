<properties
    pageTitle="Geavanceerde configuratie voor Azure Mobile Engagement Android SDK"
    description="Beschrijving van de geavanceerde configuratie-opties inclusief Android Manifest met Azure Mobile Engagement Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Geavanceerde configuratie voor Azure Mobile Engagement Android SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Deze procedure wordt beschreven hoe verschillende configuratieopties voor Azure Mobile Engagement Android apps configureren.

## <a name="prerequisites"></a>Vereisten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Vereisten voor machtiging
Sommige opties voor speciale machtigingen nodig, die hier worden vermeld voor verwijzing en in line in de specifieke functie. Deze machtigingen toevoegen aan de AndroidManifest.xml van uw project, onmiddellijk voor of na de `<application>` code.

De code van de machtiging moet zien eruit als het volgende, waarbij u de juiste machtigingen in de volgende tabel invullen.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Machtiging | Wanneer gebruikt |
| ---------- | --------- |
| INTERNET | Vereist. Voor het melden van basic |
| ACCESS_NETWORK_STATE | Vereist. Voor het melden van basic |
| RECEIVE_BOOT_COMPLETED | Vereist. Op het center meldingen worden weergegeven na het apparaat opnieuw starten |
| WAKE_LOCK | Aanbevolen. Maakt het verzamelen van gegevens bij het gebruik van Wi-Fi of wanneer het scherm is uitgeschakeld |
| TRILLEN | Dit is optioneel. Trillingen kunnen wanneer berichten worden ontvangen. |
| DOWNLOAD_WITHOUT_NOTIFICATION | Dit is optioneel. Hiermee kunt Android draaiende melding |
| WRITE_EXTERNAL_STORAGE | Dit is optioneel. Hiermee kunt Android draaiende melding |
| ACCESS_COARSE_LOCATION | Dit is optioneel. Kunnen Real-time locatie melden |
| ACCESS_FINE_LOCATION | Dit is optioneel. Kunnen op basis van GPS-locatie melden |

Beginnen met Android M, [Sommige machtigingen worden beheerd tijdens runtime](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Als u al werkt met ``ACCESS_FINE_LOCATION``, moet u niet ook ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android Manifest configuratieopties

### <a name="crash-report"></a>Rapport van crash

Schakel foutenrapporten toevoegen deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst-drempel

Standaard registreert de betrokkenheid bij de servicerapporten in realtime. Als uw rapport toepassingslogboeken vaak verschillen, is het beter de logboeken van de buffer en deze allemaal tegelijk op een regelmatige basis tijd (een zogenaamde 'burst-modus'). Daarvoor, toevoegen van deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst-modus iets verhoogt de levensduur van de batterij, maar heeft dit gevolgen voor de Monitor Engagement: duur van alle sessies en projecten worden afgerond op de drempel van burst (dus sessies en taken die korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar). De burst-drempel mag niet langer zijn dan 30000 (30s).

### <a name="session-timeout"></a>Time-out van sessie

 U kunt een activiteit beëindigen door te drukken op de toets **Home** of **terug** door de telefoon inactief of door te gaan naar een andere toepassing. Een sessie wordt standaard tien seconden na het einde van de laatste activiteit beëindigd. Zo voorkomt u een splitsing sessie telkens wanneer de gebruiker wordt afgesloten en keert u terug naar de toepassing snel, dat kan gebeuren wanneer de gebruiker een afbeelding, opvangt controleert een melding, enz. U kunt deze parameter wijzigen. Daarvoor, toevoegen van deze code tussen de `<application>` en `</application>` tags:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Melden van logboek uitschakelen

### <a name="using-a-method-call"></a>Met behulp van een aanroep van de methode

Als u wilt dat Engagement om te stoppen met het verzenden van Logboeken, kunt u bellen:

    EngagementAgent.getInstance(context).setEnabled(false);

Deze oproep is permanent: een bestand met voorkeursinstellingen voor gedeelde wordt gebruikt.

Als Engagement als u deze functie actief is, duurt een minuut om de service te stoppen. Maar het starten de service op de volgende keer dat Won't start u de toepassing.

Kunt u de rapportage opnieuw door te bellen met dezelfde functie logboek inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integratie in uw eigen`PreferenceActivity`

In plaats van deze functie aanroept, kunt u deze instelling ook integreren in uw bestaande `PreferenceActivity`.

Betrokkenheid bij uw voorkeurenbestand (met de gewenste modus) te gebruiken kunt u configureren in het `AndroidManifest.xml` het bestand met `application meta-data`:

-   De `engagement:agent:settings:name` sleutel wordt gebruikt voor de naam van het bestand met gedeelde voorkeursinstellingen definiëren.
-   De `engagement:agent:settings:mode` sleutel wordt gebruikt voor het definiëren van de modus van het gedeelde voorkeurenbestand. Dezelfde gebruikt als in de `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van vlaggen constant in uw code gebruikt, controleert u de totale waarde.

Betrokkenheid bij de altijd gebruikt de `engagement:key` boolean sleutel in het bestand met voorkeursinstellingen voor het beheer van deze instelling.

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

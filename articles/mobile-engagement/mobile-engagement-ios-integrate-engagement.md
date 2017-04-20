<properties
    pageTitle="Azure Mobile Engagement iOS SDK integratie | Microsoft Azure"
    description="Meest recente updates en procedures voor de iOS SDK voor Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Het integreren van betrokkenheid op iOS

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Deze procedure beschrijft de eenvoudigste manier om de betrokkenheid van Analytics en functies in uw toepassing iOS controle activeren.

De betrokkenheid bij de SDK vereist iOS6 + en Xcode 8: het implementatiedoel van de toepassing moet minimaal iOS 6.

> [AZURE.NOTE]
> Als u echt afhankelijk van XCode 7 zijn kunt u de [iOS SDK betrokkenheid bij de v3.2.4](https://aka.ms/r6oouh). Er is een bekende bug op de module van het bereiken van deze eerdere versie terwijl uitgevoerd op 10 iOS apparaten Zie [de integratie bereiken module](mobile-engagement-ios-integrate-engagement-reach.md) voor meer informatie. Als u de SDK-v3.2.4 gebruiken en vervolgens gewoon overslaan de `UserNotifications.framework` importeren in de volgende stap.

De volgende stappen zijn genoeg om het verslag van de logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals activeren. Het verslag van de logboeken die nodig zijn voor het berekenen van andere statistieken, zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de API Engagement (Zie [het gebruik van geavanceerde mobiele aanstelling tagging API in de iOS-app](mobile-engagement-ios-use-engagement-api.md) aangezien deze statistieken afhankelijk van de toepassing zijn.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>De betrokkenheid bij de SDK insluiten in uw iOS-project

- De iOS SDK downloaden vanaf [hier](http://aka.ms/qk2rnj).

- De betrokkenheid bij de SDK toevoegen aan uw project iOS: in Xcode, klik met de rechtermuisknop op het project en selecteer **"Add bestanden..."** en kies de `EngagementSDK` map.

- Betrokkenheid vereist extra kaders werken: in de Projectverkenner het project openen en selecteert u het juiste doel. Vervolgens opent u het tabblad **'Build fasen'** en toevoegen in het menu **"Link binaire met Libraries"** deze kaders:

    -   `UserNotifications.framework`-de koppeling als instellen`Optional`
    -   `AdSupport.framework`-de koppeling als instellen`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Het kader van de AdSupport kan worden verwijderd. Engagement moet in dit kader voor het verzamelen van de IDFA. Echter, IDFA-collectie kan worden uitgeschakeld \<ios sdk-betrokkenheid bij de idfa\> te voldoen aan het nieuwe beleid van Apple met betrekking tot deze.

##<a name="initialize-the-engagement-sdk"></a>De betrokkenheid bij de SDK worden geïnitialiseerd

U moet uw gemachtigde toepassing wijzigen:

-   Aan de bovenkant van uw implementatiebestand importeren de betrokkenheid bij de agent:

        [...]
        #import "EngagementAgent.h"

-   Betrokkenheid geïnitialiseerd in de methode '**applicationDidFinishLaunching:**"of"**-toepassing: didFinishLaunchingWithOptions:**":

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Aanbevolen methode: overbelasten uw `UIViewController` klassen

Om te activeren in het rapport van de logboekbestanden die zijn vereist voor betrokkenheid bij de gebruikers, sessies, activiteiten, Crashes en technische statistieken te berekenen, kunt u gewoon maken alle uw `UIViewController` onderliggende klassen overnemen van de `EngagementViewController` klassen (dezelfde regel voor `UITableViewController`  - \> `EngagementTableViewController`).

**Zonder afspraak:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Met afspraak:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bel `startActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `UIViewController` -klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent`van methoden direct.

> [AZURE.IMPORTANT] De iOS SDK automatisch roept de `endActivity()` methode wanneer de toepassing wordt gesloten. Dus is het *sterk* aanbevolen om aan te roepen de `startActivity` methode wanneer de activiteit van de gebruiker wijzigen, en op *nooit* roept de `endActivity` methode, omdat deze methode zorgt ervoor dat de huidige sessie moet worden beëindigd.

##<a name="location-reporting"></a>Locatie melden

Toepassingen moeten worden gebruikt voor statistische doeleinden alleen de locatietraceerfunctie toegestaan Apple servicevoorwaarden niet. Het verdient dus locatie rapporten inschakelen als voor uw toepassing ook de locatie bijhouden om een andere reden.

U moet een beschrijving van hoe uw app locatie services gebruikt door een tekenreeks voor de sleutel [NSLocationWhenInUseUsageDescription] of [NSLocationAlwaysUsageDescription] in Info.plist-bestand van uw app vanaf iOS 8, opgeven. Als u de Rapportlocatie op de achtergrond met Engagement wilt, kunt u de sleutel NSLocationAlwaysUsageDescription toevoegen. In alle andere gevallen moet u de sleutel NSLocationWhenInUseUsageDescription toevoegen.

### <a name="lazy-area-location-reporting"></a>Lazy gebied locatie melden

Lazy gebied locatie reporting kunt melden van het land, regio en plaats die zijn gekoppeld aan apparaten. Dit type locatie melden wordt alleen gebruikt voor netwerklocaties (op basis van cel-ID of Wi-Fi). Het gebied van het apparaat is gemeld maximaal eenmaal per sessie. De GPS is nooit gebruikt en dit type locatie rapport heeft slechts weinig (en niet naar Nee zeggen) dus invloed op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach campagnes. Het laatst bekende gebied gemeld voor een apparaat kan dankzij de [API apparaat]worden opgehaald.

Voeg de volgende regel na het initialiseren van de betrokkenheid bij de agent zodat lazy gebied locatie melden:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Real-time melden van locatie

Rapportage van real time locatie kan rapporteren de breedte- en lengtewaarden gekoppeld aan apparaten. Dit type locatie melden wordt alleen netwerklocaties (op basis van cel-ID of Wi-Fi) en de rapportage is alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie).

Locaties in real time worden *niet* gebruikt om statistieken te berekenen. Hun enige doel is dat het gebruik van real-time geo-fencing \<Reach-doelgroep-geofencing\> criterium in Reach campagnes.

Voeg de volgende regel na het initialiseren van de betrokkenheid bij de agent zodat real-time locatie melden:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS gebaseerde rapportage

Real-time melden van locatie alleen standaard op basis van netwerklocaties. Als u wilt dat het gebruik van op basis van GPS-locaties (die veel nauwkeuriger), voeg toe:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Achtergrond melden

Standaard is real-time melden van locatie alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie). Zodat de rapportage ook in achtergrond toevoegen:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Wanneer de toepassing op de achtergrond, wordt alleen uitgevoerd netwerk op basis van locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Implementatie van deze functie roept [startMonitoringSignificantLocationChanges] zodra de toepassing in de achtergrond wordt. Let erop dat deze automatisch de toepassing in de achtergrond start, als een nieuwe locatie gebeurtenis komt.

##<a name="advanced-reporting"></a>Geavanceerde rapportage

Optioneel, als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de betrokkenheid bij de API gebruiken via de methoden van de `EngagementAgent` klasse. Een object van deze klasse kan worden opgehaald door het aanroepen van de `[EngagementAgent shared]` statische methode.

De betrokkenheid bij de API kunt u alle geavanceerde mogelijkheden van betrokkenheid van en wordt beschreven in het gebruik van de API Engagement op iOS (en van de technische documentatie van de `EngagementAgent` klasse).

##<a name="disable-idfa-collection"></a>IDFA collectie uitschakelen

Standaard betrokkenheid van de [IDFA] gebruikt als unieke identificatie voor een gebruiker. Maar als u niet ergens anders in de app reclame, kan u geweigerd door het controleproces App Store. IDFA-collectie kan worden uitgeschakeld door de preprocessor macro toe te voegen `ENGAGEMENT_DISABLE_IDFA` in het bestand pch (of in de `Build Settings` van de toepassing). Dit zal ervoor zorgen dat er geen verwijzingen naar `ASIdentifierManager`, `advertisingIdentifier` of `isAdvertisingTrackingEnabled` bij het opbouwen van toepassing.

Integratie in het bestand **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

U kunt controleren dat de IDFA-collectie wordt uitgeschakeld in uw toepassing door de betrokkenheid bij de test logboeken controleren. Zie de integratie testen\<i/o-sdk-engagement-test-idfa\> documentatie voor meer informatie.

##<a name="disable-log-reporting"></a>Melden van logboek uitschakelen

### <a name="using-a-method-call"></a>Met behulp van een aanroep van de methode

Als u wilt dat Engagement om te stoppen met het verzenden van Logboeken, kunt u bellen:

    [[EngagementAgent shared] setEnabled:NO];

Deze oproep is permanent: gebruikt `NSUserDefaults` de gegevens op te slaan.

Kunt u de rapportage opnieuw door te bellen met dezelfde functie logboek inschakelen `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integratie in uw bundel instellingen

In plaats van deze functie aanroept, kunt u deze instelling ook integreren in uw bestaande `Settings.bundle` bestand. De tekenreeks `engagement_agent_enabled` moet worden gebruikt als een, en de voorkeur-id moeten worden gekoppeld aan een aan/uit-schakelaar (`PSToggleSwitchSpecifier`).

In het volgende voorbeeld van `Settings.bundle` ziet u hoe u deze toepast:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[API-apparaat]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier

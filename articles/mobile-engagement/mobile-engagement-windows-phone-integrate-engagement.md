<properties 
    pageTitle="Integratie van Windows Phone Silverlight Engagement SDK" 
    description="Betrokkenheid bij de mobiele Azure integreren met Windows Phone Silverlight Apps"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Integratie van Windows Phone Silverlight Engagement SDK

> [AZURE.SELECTOR] 
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Deze procedure beschrijft de eenvoudigste manier Azure Mobile Engagement Analytics en controle van de functies in de Windows Phone Silverlight-toepassing te activeren.

De volgende stappen zijn genoeg om het verslag van de logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals activeren. Het verslag van de logboeken die nodig zijn voor het berekenen van andere statistieken, zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de API Engagement (Zie hieronder [het gebruik van geavanceerde mobiele aanstelling API in uw Windows Phone Silverlight app tagging](mobile-engagement-windows-phone-use-engagement-api.md) ) omdat deze statistieken afhankelijk van de toepassing zijn.

##<a name="supported-versions"></a>Ondersteunde versies

De SDK met Engagement Mobile voor Windows Silverlight kunnen alleen worden geïntegreerd in toepassingen die zijn gericht op:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Als u Windows Phone 8.1 (niet-Silverlight) vervolgens verwijzen naar de [universele Windows integratie procedure](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>De betrokkenheid bij de mobiele Silverlight SDK installeren

De SDK met Engagement Mobile voor Windows Silverlight is beschikbaar als een Nuget package *MicrosoftAzure.MobileEngagement*genoemd. U kunt het installeren vanuit de Visual Studio Nuget Package Manager. 

##<a name="add-the-capabilities"></a>Voeg de functies

De betrokkenheid bij de SDK nodig sommige mogelijkheden voor de goede werking van de Windows Phone Silverlight SDK.

Open de `WMAppManifest.xml` -bestand en zorg ervoor dat de volgende mogelijkheden zijn gedefinieerd in de `Capabilities` Configuratiescherm:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>De betrokkenheid bij de SDK worden geïnitialiseerd

### <a name="engagement-configuration"></a>Betrokkenheid bij de configuratie

De betrokkenheid bij de configuratie is gecentraliseerd het `Resources\EngagementConfiguration.xml` -bestand van uw project.

Bewerk dit bestand op te geven:

-   De verbindingsreeks van toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u wilt dat in plaats daarvan tijdens de uitvoering te geven, kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="engagement-initialization"></a>Betrokkenheid bij de initialisatie

Wanneer u een nieuw project maakt een `App.xaml.cs` bestand is gegenereerd. Deze klasse overerft van `Application` en bevat veel belangrijke methoden. Het wordt ook worden gebruikt voor het initialiseren van de betrokkenheid bij de SDK.

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Voeg `EngagementAgent.Instance.Init` in de `Application_Launching` methode:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Voeg `EngagementAgent.Instance.OnActivated` in de `Application_Activated` methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] Wij ontmoedigen raden u de initialisatie van de overeenkomst op een andere plaats van uw toepassing toevoegen. Wel rekening houden dat de `EngagementAgent.Instance.Init` methode wordt uitgevoerd op een speciale thread, en niet op de UI-thread.

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Aanbevolen methode: overbelasten uw `PhoneApplicationPage` klassen

Om te activeren in het rapport van de logboekbestanden die zijn vereist voor betrokkenheid bij de gebruikers, sessies, activiteiten, Crashes en technische statistieken te berekenen, kunt u gewoon maken alle uw `PhoneApplicationPage` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe dit voor een pagina van uw toepassing. U kunt hetzelfde doen voor alle pagina's van uw toepassing.

#### <a name="c-source-file"></a>C#-bronbestand

Uw pagina wijzigen `.xaml.cs` bestand:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Vervangen `PhoneApplicationPage` met `EngagementPage` :

**Zonder afspraak:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Met afspraak:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Als uw pagina van overneemt de `OnNavigatedTo` methode, zorg ervoor dat u laat de `base.OnNavigatedTo(e)` aanroepen. Anders wordt de activiteit niet gemeld. Inderdaad, de `EngagementPage` is het aanroepen van `StartActivity` in de `OnNavigatedTo` methode.

#### <a name="xaml-file"></a>XAML bestand

Uw pagina wijzigen `.xaml` bestand:

-   Voeg toe aan de declaraties van naamruimten:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Vervangen `phone:PhoneApplicationPage` met `engagement:EngagementPage` :

**Zonder afspraak:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Met afspraak:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Het standaardgedrag overschrijven

Standaard wordt de naam van de klasse van de pagina als de naam van de activiteit met zonder extra vermeld. Als de klasse worden gebruikt het achtervoegsel "Pagina", Engagement worden dan ook verwijderd.

Als u het standaardgedrag voor de naam overschrijven wilt, gewoon toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Als u wilt dat sommige extra informatie aan uw activiteit te melden, kunt u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bel `StartActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `PhoneApplicationPage` -klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` methoden direct.

Beste telefoonkaart `StartActivity` in de `OnNavigatedTo` methode van de PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Zorg ervoor dat u uw sessie beëindigen correct.
>
> De SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **sterk** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen, en op **nooit** roept de `EndActivity` methode. Deze methode verzendt een bericht naar de server Engagement dat de huidige gebruiker de toepassing heeft verlaten, en dit heeft gevolgen voor alle toepassingslogboeken.

##<a name="advanced-reporting"></a>Geavanceerde rapportage

U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, hiervoor gebruik eventueel de andere methoden worden gevonden in de `EngagementAgent` klasse. De betrokkenheid-API kunt gebruiken alle betrokkenheid van geavanceerde mogelijkheden.

Zie [het gebruik van geavanceerde mobiele aanstelling tagging API in uw Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md)voor meer informatie.

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Uitschakelen automatische foutenrapportage

U kunt de automatische crash rapportage functie van betrokkenheid uitschakelen. Vervolgens, wanneer zich een niet-verwerkte uitzondering Engagement niet alles.

> [AZURE.WARNING] Als u deze functie uitschakelt, worden er rekening mee houden dat wanneer een niet-afgehandelde crash in uw app plaatsvinden zal, Engagement stuurt het vastlopen **en** deze sessie en taken niet gesloten.

Pas de configuratie van de manier waarop die u verklaard dat deze Schakel automatische crash rapportage:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand

Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime

Rapport crash ingesteld op false, het EngagementConfiguration-object gebruiken.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-modus

Standaard registreert de betrokkenheid bij de servicerapporten in realtime. Als de toepassing Logboeken zeer vaak wordt gemeld, is het beter in de buffer van de logboeken en om deze in één keer op een vaste tijd basis (dit is de 'burst-modus' genoemd) te melden.

Bel hiervoor de methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Op elk gewenst moment, als u activeren de real-time logboekregistratie wilt, net roept u de methode geen parameters opgeeft, of met de waarde 0.

De burst-modus iets langere levensduur, maar heeft een impact op de betrokkenheid bij de Monitor: duur van alle sessies en taken aan de drempelwaarde burst (sessies en dus korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken) worden afgerond. Het verdient aanbeveling een burst drempelwaarde niet meer dan 30000 (30s) gebruiken. U moet rekening houden dat opgeslagen logboeken zijn beperkt tot 300 artikelen. U kunt sommige logs verliezen als verzenden te lang is.

> [AZURE.WARNING] De drempel burst kan niet worden geconfigureerd voor een periode van minder dan één seconde. Als u probeert te doen, zodat de SDK traceren met de fout en wordt automatisch ingesteld op de standaardwaarde wordt weergegeven, dat wil zeggen nul seconden. Dit activeert de SDK om te rapporteren in de logboekbestanden in real-time.
 

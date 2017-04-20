<properties 
    pageTitle="Integratie van Windows Universal Apps Engagement SDK" 
    description="Betrokkenheid bij de mobiele Azure integreren met Windows Universal Apps"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Integratie van Windows Universal Apps Engagement SDK

> [AZURE.SELECTOR] 
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Silverlight voor Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android](mobile-engagement-android-integrate-engagement.md) 

Deze procedure beschrijft de eenvoudigste manier om de betrokkenheid van Analytics en de controle op functies in de Windows-universele toepassing activeren.

De volgende stappen zijn genoeg om het verslag van de logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals activeren. Het verslag van de logboeken die nodig zijn voor het berekenen van andere statistieken, zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de API Engagement (Zie [het gebruik van geavanceerde mobiele aanstelling tagging API in uw Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) aangezien deze statistieken afhankelijk van de toepassing zijn.

## <a name="supported-versions"></a>Ondersteunde versies

De Mobile Engagement SDK voor Windows Universal Apps kunnen alleen worden geïntegreerd in Windows Runtime en Universal Windows Platform toepassingen maken voor:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows 10 (desktop- en mobiele families)

> [AZURE.NOTE] Als u Windows Phone Silverlight willen verwijzen naar de [procedure voor Windows Phone Silverlight-integratie](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>De betrokkenheid bij de mobiele Universal Apps SDK installeren

### <a name="all-platforms"></a>Alle platforms

De Mobile Engagement SDK voor Windows Universal App is beschikbaar als een Nuget package *MicrosoftAzure.MobileEngagement*genoemd. U kunt het installeren vanuit de Visual Studio Nuget Package Manager.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x en Windows Phone 8.1

NuGet automatisch wordt uitgevoerd, implementeert de SDK-bronnen in de `Resources` map in de hoofdmap van het toepassingsproject.

### <a name="windows-10-universal-windows-platform-applications"></a>Toepassingen voor Windows 10 Universal Windows Platform

NuGet implementeert de SDK-bronnen in uw toepassing UWP nog geen automatisch. U moet dit handmatig doen totdat de implementatie van resources wordt binnengebracht in NuGet:

1.  Open de Verkenner.
2.  Ga naar de volgende locatie (**x.x.x** is de versie van de afspraak die u installeert): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Slepen en neerzetten van de map **bronnen** uit de Verkenner naar de hoofdmap van het project in Visual Studio.
4.  Selecteer uw project in Visual Studio en activeren van het pictogram **weergegeven van alle bestanden** op de **Solution Explorer**.
5.  Sommige bestanden worden niet opgenomen in het project. Als u wilt importeren Klik ze in één keer met de rechtermuisknop op de map **bronnen** , **uitsluiten van het project** en vervolgens een ander Klik met de rechtermuisknop op de map **bronnen** , **opnemen in het project** opnieuw ook de hele map. Alle bestanden uit de map **Resources** zijn nu opgenomen in uw project.

## <a name="add-the-capabilities"></a>Voeg de functies

De betrokkenheid bij de SDK moet enkele mogelijkheden voor de goede werking van de SDK van Windows.

Open de `Package.appxmanifest` -bestand en zorg ervoor dat de volgende mogelijkheden worden aangegeven:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>De betrokkenheid bij de SDK worden geïnitialiseerd

### <a name="engagement-configuration"></a>Betrokkenheid bij de configuratie

De betrokkenheid bij de configuratie is gecentraliseerd het `Resources\EngagementConfiguration.xml` -bestand van uw project.

Bewerk dit bestand op te geven:

-   De verbindingsreeks van toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u wilt dat in plaats daarvan tijdens de uitvoering te geven, kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie bellen:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="engagement-initialization"></a>Betrokkenheid bij de initialisatie

Wanneer u een nieuw project maakt een `App.xaml.cs` bestand is gegenereerd. Deze klasse overerft van `Application` en bevat veel belangrijke methoden. Het wordt ook worden gebruikt voor het initialiseren van de betrokkenheid bij de SDK.

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Een methode voor het delen van de betrokkenheid bij de initialisatie eenmaal voor alle gesprekken definiëren:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Bel `InitEngagement` in de `OnLaunched` methode:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Wanneer de toepassing wordt gestart met een aangepast schema, een andere toepassing of de opdrachtregel en vervolgens de `OnActivated` methode wordt aangeroepen. U moet ook de betrokkenheid van SDK initiëren wanneer uw toepassing wordt geactiveerd. Hiertoe negeren `OnActivated` methode:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] Wij ontmoedigen raden u de initialisatie van de overeenkomst op een andere plaats van uw toepassing toevoegen.

## <a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelasten uw `Page` klassen

Om te activeren in het rapport van de logboekbestanden die zijn vereist voor betrokkenheid bij de gebruikers, sessies, activiteiten, Crashes en technische statistieken te berekenen, kunt u gewoon maken alle uw `Page` onderliggende klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe dit voor een pagina van uw toepassing. U kunt hetzelfde doen voor alle pagina's van uw toepassing.

#### <a name="c-source-file"></a>C#-bronbestand

Uw pagina wijzigen `.xaml.cs` bestand:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Vervangen `Page` met `EngagementPage`:

**Zonder afspraak:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Met afspraak:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Als uw pagina overschrijft de `OnNavigatedTo` -methode moet aanroepen `base.OnNavigatedTo(e)`. Anders wordt de activiteit niet worden gerapporteerd (de `EngagementPage` oproepen `StartActivity` in de `OnNavigatedTo` methode).

#### <a name="xaml-file"></a>XAML bestand

Uw pagina wijzigen `.xaml` bestand:

-   Voeg toe aan de declaraties van naamruimten:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Vervangen `Page` met `engagement:EngagementPage`:

**Zonder afspraak:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Met afspraak:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Standaard overschrijven

Standaard wordt de naam van de klasse van de pagina als de naam van de activiteit met zonder extra vermeld. Als de klasse worden gebruikt het achtervoegsel "Pagina", Engagement worden dan ook verwijderd.

Als u overschrijven de standaardwerking voor de naam wilt, gewoon toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Als u rapporteren sommige extra informatie over uw activiteit wilt, kunt u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bel `StartActivity()` handmatig

Als u niet kan of wil niet overbelasten uw `Page` -klassen, in plaats daarvan kunt u uw activiteiten starten door het aanroepen van `EngagementAgent` methoden direct.

We raden je aan het bellen `StartActivity` in de `OnNavigatedTo` methode van de pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Zorg ervoor dat u uw sessie beëindigen correct.
> 
> Universele Windows SDK automatisch roept de `EndActivity` methode wanneer de toepassing wordt gesloten. Het is dus **sterk** aanbevolen om aan te roepen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen, en op **nooit** roept de `EndActivity` methode deze methode verzendt naar betrokkenheid bij de server dat de huidige gebruiker de toepassing heeft verlaten, dit zal alle toepassingslogboeken invloed.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, hiervoor gebruik eventueel de andere methoden worden gevonden in de `EngagementAgent` klasse. De betrokkenheid-API kunt gebruiken alle betrokkenheid van geavanceerde mogelijkheden.

Zie voor meer informatie, [het gebruik van geavanceerde mobiele aanstelling tagging API in de universele Windows app](mobile-engagement-windows-store-use-engagement-api.md).

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Uitschakelen automatische foutenrapportage

U kunt de automatische crash rapportage functie van betrokkenheid uitschakelen. Vervolgens, wanneer zich een niet-verwerkte uitzondering Engagement niet alles.

> [AZURE.WARNING] Als u deze functie uitschakelt, worden er rekening mee houden dat wanneer een niet-afgehandelde crash in uw app plaatsvinden zal, Engagement stuurt dat het vastlopen **en** de taken en de sessie wordt afgesloten.

Pas de configuratie van de manier waarop die u verklaard dat deze Schakel automatische crash rapportage:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand

Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime

Rapport crash ingesteld op false, het EngagementConfiguration-object gebruiken.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-modus

Standaard registreert de betrokkenheid bij de servicerapporten in realtime. Als de toepassing Logboeken zeer vaak wordt gemeld, is het beter in de buffer van de logboeken en om deze in één keer op een vaste tijd basis (dit is de 'burst-modus' genoemd) te melden.

Bel hiervoor de methode:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Op elk gewenst moment, als u activeren de real-time logboekregistratie wilt, net roept u de methode geen parameters opgeeft, of met de waarde 0.

De burst-modus iets langere levensduur, maar heeft een impact op de betrokkenheid bij de Monitor: duur van alle sessies en taken aan de drempelwaarde burst (sessies en dus korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken) worden afgerond. Het verdient aanbeveling een burst drempelwaarde niet meer dan 30000 (30s) gebruiken. U moet rekening houden dat opgeslagen logboeken zijn beperkt tot 300 artikelen. U kunt sommige logs verliezen als verzenden te lang is.

> [AZURE.WARNING] De burst-drempel kan niet worden geconfigureerd voor een periode van minder dan 1s. Als u dit probeert, wordt de SDK traceren met de fout wordt weergegeven en wordt automatisch opnieuw ingesteld op de standaardwaarde, dat wil zeggen: 0s. Dit activeert de SDK om te rapporteren in de logboekbestanden in real-time.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 

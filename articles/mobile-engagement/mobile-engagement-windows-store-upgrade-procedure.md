<properties 
    pageTitle="Windows SDK van Universal Apps upgradeprocedures" 
    description="Universal Apps voor Windows SDK bijwerken Procedures voor betrokkenheid bij de mobiele Azure"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Windows SDK van Universal Apps upgradeprocedures

Als u hebt een oudere versie van de overeenkomst al geïntegreerd in uw toepassing, moet u rekening houden met de volgende punten bij het upgraden van de SDK.

Mogelijk moet verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 0.10.1 naar 0.11.0 die u moet eerst volgt u de procedure "van 0.9.0 naar 0.10.1' vervolgens de procedure"van 0.10.1 naar 0.11.0' bijvoorbeeld.

##<a name="from-330-to-340"></a>Van 3.3.0 naar 3.4.0

### <a name="test-logs"></a>Testen van Logboeken

Console logs geproduceerd door de SDK kunnen nu worden ingeschakeld/uitgeschakeld/gefilterd. De eigenschap voor bijwerken als u wilt aanpassen, `EngagementAgent.Instance.TestLogEnabled` op een van de waarde van de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Bronnen

De Reach-overlay is verbeterd. Het uitmaakt deel van de SDK NuGet package bronnen.

Tijdens het upgraden naar de nieuwe versie van de SDK die u kunt kiezen of u uw bestaande bestanden uit de map bedekking van uw resources houden wilt of niet:

* Als de vorige overlay voor u werkt of u integreert de `WebView` -elementen handmatig klikt u kunt uw bestaande bestanden, het werkt nog steeds. 
* Als u bijwerken naar de nieuwe overlay wilt, vervangt de volledige `overlay` map van uw resources door de nieuwe uit de SDK-pakket (UWP apps: na de upgrade, kunt u de nieuwe overlay map ophalen van % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Met behulp van de nieuwe overlay worden overschreven door eventuele aanpassingen die zijn aangebracht in de vorige versie.

##<a name="from-320-to-330"></a>Van 3.2.0 naar 3.3.0

### <a name="resources"></a>Bronnen
Deze stap betreft alleen aangepaste middelen. Als u de bronnen die door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en de aanpassingen op bijgewerkte bronnen opnieuw.

##<a name="from-310-to-320"></a>Van 3.1.0 naar 3.2.0

### <a name="resources"></a>Bronnen
Deze stap betreft alleen aangepaste middelen. Als u de bronnen die door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en de aanpassingen op bijgewerkte bronnen opnieuw.

### <a name="webview-integration"></a>Webweergave integratie
Sommige verbeteringen aan een ander apparaat vormfactoren zijn in deze versie geïntroduceerd. Zorg ervoor dat de integratie van de webweergave overeenkomen met het volgende:

In uw pagina-(XAML):

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

En in de bijbehorende:. cs-bestand:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Van 2.0.0 aan 3.0.0

### <a name="resources"></a>Bronnen
Deze stap betreft alleen aangepaste middelen. Als u de bronnen die door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze back-up voordat u de upgrade en de aanpassingen op bijgewerkte bronnen opnieuw.

##<a name="from-111-to-200"></a>Van 1.1.1 tot 2.0.0

Hieronder wordt beschreven hoe u voor het migreren van een SDK-integratie van de Capptain-service aangeboden door Capptain SAS in een aangedreven door betrokkenheid bij de Azure Mobile app. 

> [Azure.IMPORTANT] Capptain en Mobile Engagement zijn niet dezelfde diensten en de onderstaande procedure alleen markeert het migreren van de clienttoepassing. Migreren van de SDK in de app wordt niet migreren van uw gegevens de Capptain naar de servers van Mobile Engagement

Als u een migratie vanuit een eerdere versie, neem contact op met de Capptain-website om te migreren naar 1.1.1 eerst en vervolgens de volgende procedure van toepassing

### <a name="nuget-package"></a>Nuget package

**Capptain.WindowsPhone** vervangen door **MicrosoftAzure.MobileEngagement** Nuget package.

### <a name="applying-mobile-engagement"></a>Betrokkenheid bij de mobiele toepassing

De SDK wordt de term `Engagement`. U moet bijwerken van uw project zodat deze overeenkomt met deze wijziging.

U moet uw huidige Capptain nuget pakket verwijderen. U kunt alle wijzigingen in de map Capptain bronnen worden verwijderd. Als u wilt behouden die bestanden vervolgens een kopie maken van hen.

Daarna het nieuwe Microsoft Azure Engagement nuget pakket te installeren op uw project. U vindt deze rechtstreeks op [website nuget]. of hier index. Deze actie vervangt alle bronnen bestanden gebruikt door betrokkenheid en wordt de nieuwe betrokkenheid van DLL-bestand toegevoegd aan de projectverwijzingen.

U hebt de projectverwijzingen opschonen door het verwijderen van verwijzingen naar DLL Capptain. Als u dat niet doet, wordt de versie van Capptain in conflict en fouten gebeurt.

Als u Capptain resources hebt aangepast, de inhoud van uw oude bestanden kopiëren en plakken in de nieuwe overeenkomst. Houd er rekening mee dat zowel cs als xaml-bestanden moeten worden bijgewerkt.

Wanneer deze stappen zijn voltooid hebt u alleen oude Capptain verwijzingen door de nieuwe betrokkenheid verwijzingen moeten worden vervangen.

1. Alle Capptain naamruimten moeten worden bijgewerkt.

    Voor de migratie:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Na de migratie:
    
        using Microsoft.Azure.Engagement;

2. Alle klassen van Capptain met 'Capptain' moeten 'Engagement' bevatten.

    Voor de migratie:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Na de migratie:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Xaml-bestanden wijzigen Capptain naamruimte en kenmerken ook.

    Voor de migratie:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Na de migratie:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Overlay pagina wijzigingen
    > [AZURE.IMPORTANT] Overlay wordt ook gewijzigd. De nieuwe naamruimte is `Microsoft.Azure.Engagement.Overlay`. Het heeft in xaml- en cs-bestanden worden gebruikt. Bovendien `CapptainGrid` is de naam `EngagementGrid`, `capptain_notification_content` en `capptain_announcement_content` zijn met de naam `engagement_notification_content` en `engagement_announcement_content`.
    
    Voor de overlay:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Dit wordt:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Voor andere bronnen zoals Capptain, afbeeldingen en HTML-bestanden, houd er rekening mee dat zij ook hebben gekregen voor het gebruik van 'Engagement'.

### <a name="project-declaration"></a>Verklaring van project

Op Package.appxmanifest `File Type Associations` is bijgewerkt op basis van:

 -   capptain\_tot\_inhoud naar betrokkenheid bij de\_tot\_inhoud
 -   capptain\_log\_bestand betrokkenheid\_log\_bestand

### <a name="application-id--sdk-key"></a>Toepassings-ID / sleutel SDK

Betrokkenheid van een verbindingsreeks gebruikt. U hoeft niet een toepassings-ID en een sleutel SDK opgeven bij Mobile Engagement, hoeft u alleen een verbindingsreeks opgeven. U kunt instellen op het bestand EngagementConfiguration.

De betrokkenheid bij de configuratie kan worden ingesteld in de `Resources\EngagementConfiguration.xml` -bestand van uw project.

Bewerk dit bestand op te geven:

-   De verbindingsreeks van toepassing tussen de tags `<connectionString>` en `<\connectionString>`.

Als u wilt dat in plaats daarvan tijdens de uitvoering te geven, kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

De verbindingsreeks voor de toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="items-name-change"></a>Naam wijzigen van items

Alle items met de naam *capptain* zijn *engagement*genoemd. Op dezelfde manier voor *Capptain* aan *betrokkenheid*.

Voorbeelden van veelgebruikte Capptain items:

-   CapptainConfiguration nu met de naam EngagementConfiguration
-   CapptainAgent nu met de naam EngagementAgent
-   CapptainReach nu met de naam EngagementReach
-   CapptainHttpConfig nu met de naam EngagementHttpConfig
-   GetCapptainPageName nu met de naam GetEngagementPageName

Opmerking deze naam ook overschreven methoden betreft.

 

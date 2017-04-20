<properties 
    pageTitle="Universal Apps voor Windows bereiken SDK integratie" 
    description="Azure mobiele Engagement Reach integreren met Windows Universal Apps"
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

# <a name="windows-universal-apps-reach-sdk-integration"></a>Universal Apps voor Windows bereiken SDK integratie

U moet de integratie-procedure in de [Integratie van Windows Universal Engagement SDK](mobile-engagement-windows-store-integrate-engagement.md) voordat u deze handleiding volgen.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>De SDK voor het bereiken van betrokkenheid insluiten in uw Windows Universal project

U hoeft niet iets toe te voegen. `EngagementReach`verwijzingen en bronnen zijn al in uw project.

> [AZURE.TIP] U kunt afbeeldingen uit de `Resources` map van uw project, met name het merk-pictogram (dat standaard het pictogram betrokkenheid). Op de Universal Apps kunt u ook verplaatsen de `Resources` map op het gedeelde project te delen tussen toepassingen, maar de inhoud ervan moet houden de `Resources\EngagementConfiguration.xml` -bestand in de standaardlocatie platform afhankelijk is.

## <a name="enable-the-windows-notification-service"></a>De Windows Notification-Service inschakelen

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x en Windows Phone 8.1 alleen

De **Windows Notification Service** (WNS genoemd) gebruiken uw `Package.appxmanifest` -bestand in de `Application UI` Klik op `All Image Assets` in het vak links bot. Aan de rechterkant van het vak in `Notifications`, wijzigen `toast capable` van `(not set)` op `(Yes)`.

### <a name="all-platforms"></a>Alle platforms

U moet uw app in uw Microsoft-account en het engagement platform te synchroniseren. Daarvoor moet u een account aanmaken of aanmelden [dev center van windows](https://dev.windows.com). Daarna een nieuwe toepassing maken en de SID en de geheime sleutel te vinden. Ga op de frontend betrokkenheid van uw instelling app in `native push` en uw referenties te plakken. Daarna, klik met de rechtermuisknop op uw project, selecteer `store` en `Associate App with the Store...`. U hoeft alleen maar de toepassing maken voordat het synchroniseren.

## <a name="initialize-the-engagement-reach-sdk"></a>Initialiseren van de betrokkenheid bij de Reach-SDK

Wijzig de `App.xaml.cs`:

-   Voeg `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in de `InitEngagement` methode:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    De `EngagementReach.Instance.Init` in een speciale thread wordt uitgevoerd. U hoeft niet zelf doen.

> [AZURE.NOTE] Als u met push-meldingen elders in uw toepassing hebt u [uw push-kanaal deelt](#push-channel-sharing) met het Engagement bereiken.

## <a name="integration"></a>Integratie

Betrokkenheid kent twee methoden voor het toevoegen van de Reach-app banners en verspreide weergaven voor aankondigingen en stemmingen in uw toepassing: de overlay-integratie en de handmatige integratie van web-weergaven. U moet niet beide aanpak op dezelfde pagina combineren.

De keuze tussen de twee integratie kan op deze manier worden samengevat:

-   U kunt de overlay integratie als uw pagina's al worden overgenomen van de Agent `EngagementPage`, het is gewoon een kwestie van het vervangen van `EngagementPage` door `EngagementPageOverlay` en `xmlns:engagement="using:Microsoft.Azure.Engagement"` door `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in uw pagina's.
-   U kunt de webweergaven handmatige integratie als plaats nauwkeurig de UI bereiken in uw pagina's u wilt of als u niet wilt dat een ander overnameniveau toevoegen aan uw pagina's. 

### <a name="overlay-integration"></a>Overlay-integratie

De betrokkenheid bij de overlay wordt dynamisch de gebruikersinterface-elementen gebruikt om campagnes bereiken in uw pagina weer te geven. Als de overlay niet aanpassen aan uw lay-out moet u overwegen de webweergaven handmatige integratie in plaats daarvan.

In het bestand .xaml wijzigen `EngagementPage` de verwijzing naar`EngagementPageOverlay`

-   Voeg toe aan de declaraties van naamruimten:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Vervangen `engagement:EngagementPage` met `engagement:EngagementPageOverlay`:

**Met EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Met EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Klik in het bestand:. cs tag uw pagina in `EngagementPageOverlay` in plaats van `EngagementPage` en `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Vervangen `EngagementPage` met `EngagementPageOverlay`:

**Met EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Met EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


De betrokkenheid bij de overlay worden toegevoegd een `Grid` element op de pagina bestaat uit uw lay-out en de twee `WebView` elementen, één voor de banner en een voor de verspreide weergave.

U kunt de overlay elementen rechtstreeks in de `EngagementPageOverlay.cs` bestand.

### <a name="web-views-manual-integration"></a>Handmatige webintegratie weergaven

Bereik zoeken op uw pagina's voor de twee naar `WebView` elementen die verantwoordelijk is voor het weergeven van de banner en de verspreide weergave. Het enige wat u hoeft te doen is het toevoegen van deze twee `WebView` elementen ergens in uw pagina's, hier is een voorbeeld:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In dit voorbeeld de `WebView` elementen aanpassen aan hun container die automatisch opnieuw in het geval van wijziging van scherm rotatie of venster grootte worden uitgerekt.

> [AZURE.WARNING] Het is belangrijk dat het dezelfde naming `engagement_notification_content` en `engagement_announcement_content` voor de `WebView` elementen. Reach is identificeren met hun naam. 

## <a name="handle-datapush-optional"></a>Greep datapush (optioneel)

Als u wilt dat uw toepassing kunnen dit gereedschap duwt Reach gegevens ontvangen, hebt u twee gebeurtenissen van de klasse EngagementReach implementeren:

In de App.xaml.cs in de constructor App() toevoegen:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };
            
            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

U kunt zien dat het terugbellen van elke methode een Boolean-waarde retourneert. Betrokkenheid stuurt een feedback naar de back-end na verzending van de gegevens-push. Als u de terugbelfunctie false retourneert, de `exit` feedback verzenden zal zijn. Anders worden `action`. Als er geen terugbellen is ingesteld voor de gebeurtenissen, de `drop` feedback keert terug naar afspraak.

> [AZURE.WARNING] Betrokkenheid kan geen veelvouden feedback voor een push gegevens ontvangen. Als u van plan bent meerdere handlers instellen voor een gebeurtenis, houd er rekening mee dat de feedback op de laatste overeen dat een verzonden. In dit geval wordt aangeraden om altijd retourneert dezelfde waarde niet hoeft lastig feedback op de front-end.

## <a name="customize-ui-optional"></a>Aanpassen van de gebruikersinterface (optioneel)

### <a name="first-step"></a>Eerste stap

We laten u het bereik van de gebruikersinterface aanpassen.

Hiervoor hebt u voor het maken van een subklasse van de `EngagementReachHandler` klasse.

**Voorbeeld van Code:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met het aangepaste object in uw `App.xaml.cs` klasse binnen de `App()` methode.

**Voorbeeld van Code:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Betrokkenheid bij de standaard in de eigen implementatie van `EngagementReachHandler`.
> U hoeft niet uw eigen maken en als u dit doet, hoeft u niet elke methode overschrijven. Het standaardgedrag is het basisobject betrokkenheid te selecteren.

### <a name="web-view"></a>Weergave op het web

Standaard bereik ingesloten bronnen van het DLL-bestand gebruikt om de berichten en pagina's weer te geven.

Om een volledige aanpassing mogelijkheden gebruiken we alleen weergave op het web. Als u aanpassen van lay-outs wilt, overschrijven rechtstreeks bestanden bronnen `EngagementAnnouncement.html` en `EngagementNotification.html`. Betrokkenheid moet u alle programmacode in `<body></body>` goed te laten werken. Maar u kunt de buitenste tag toevoegen `engagement_webview_area`.

U kunt echter uw eigen resources gebruiken.

U kunt overschrijven `EngagementReachHandler` methoden in de subklasse vertellen betrokkenheid van uw indelingen gebruiken, maar wees voorzichtig met het mechanisme voor betrokkenheid bij de ingesloten:

**Voorbeeld van Code:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


AnnouncementHTML is standaard `ms-appx-web:///Resources/EngagementAnnouncement.html`. Hiermee geeft u het HTML-bestand dat de inhoud van een push-bericht (tekst aankondiging, Web anoucement en peiling aankondiging) ontwerpt. AnnouncementName is `engagement_announcement_content`. Dit is de naam van het ontwerp van de webweergave in de xaml-pagina.

NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`. Hiermee geeft u het HTML-bestand dat de melding van een push-bericht ontwerpen. NotfificationName is `engagement_notification_content`. Dit is de naam van het ontwerp van de webweergave in de xaml-pagina.

### <a name="customization"></a>Aanpassing

U kunt de kennisgeving en bekendmaking webweergave heeft u wilt als u Engagement object behouden. Maatregelen treffen die webweergave object driemaal - het eerst in de xaml, tweede keer in het bestand:. cs in de methode 'setwebview()' en derde keer in het HTML-bestand wordt beschreven.

-   De huidige grafische indeling webweergave onderdeel wordt beschreven in de xaml.
-   In het bestand:. cs kunt u "setwebview()", waarin u de afmetingen van de twee Webweergave (kennisgeving, aankondiging) instellen. Het is zeer effectief wanneer de toepassing wordt het formaat van.
-   De betrokkenheid bij de HTML-bestanden worden beschreven de webweergave inhoud, ontwerp en de posities van de elementen onderling.

### <a name="launch-message"></a>Bericht starten

Wanneer een gebruiker klikt op een kennisgeving van systeem (toast), betrokkenheid van de toepassing start, de inhoud van de push-berichten te laden en weergeven van de pagina voor de desbetreffende campagne.

Er is een vertraging tussen het starten van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van het netwerk).

Om aan te geven voor de gebruiker iets wordt geladen, moet u zelf een visuele informatie, zoals een voortgangsbalk of een voortgangsindicator. Betrokkenheid kan niet zelf, maar een paar handlers biedt u verwerken.

Implementatie van de callback App.xaml.cs in "Openbare App() {}" Voeg toe aan:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
            
            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
            
            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

U kunt de retouraanroep instellen in uw "Openbare App() {}" methode van uw `App.xaml.cs` bestand, bij voorkeur vóór de `EngagementReach.Instance.Init()` aanroepen.

> [AZURE.TIP] Elke-handler wordt aangeroepen door de UI-Thread. U hoeft geen zorgen te maken wanneer u een MessageBox of iets UI gerelateerd.

##<a id="push-channel-sharing"></a>Push-kanalen delen

Als u push-meldingen voor een ander doel in uw toepassing hebt u de functie van de betrokkenheid bij de SDK voor het delen push-kanaal. Dit is om te voorkomen dat de gemiste push.

- U kunt uw eigen kanaal push voor de initialisatie van de overeenkomst bereiken leveren. De SDK wordt gebruikt in plaats van een nieuwe aanvragen.

De initialisatie van de overeenkomst bereiken bijwerken met de push-kanaal in de `InitEngagement` methode van de `App.xaml.cs` bestand:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- Ook als u alleen verbruiken het push-kanaal wilt nadat u vervolgens de initialisatie Reach een retouraanroep ingesteld op het Engagement om het kanaal push eenmaal bereikt wordt deze gemaakt door de SDK.

Uw terugbellen instellen op een willekeurige plaats **nadat** de initialisatie van de bereiken:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Tip aangepaste schema

We bieden aangepaste schema gebruiken. Ander type URI kunt u verzenden van betrokkenheid bij de frontend voor gebruik in uw betrokkenheid bij de toepassing. Standaardschema als `http, ftp, ...` zijn beheren door Windows, een venster wordt gevraagd als ze geen standaard-toepassing geïnstalleerd op het apparaat. U kunt ook uw eigen aangepaste schema maken voor uw toepassing.

De eenvoudige manier om een aangepast schema instellen in uw toepassing is voor het openen van de `Package.appxmanifest` gaat in `Declarations` paneel. Selecteer `Protocol` schuiven in de beschikbare verklaringen vak en toe te voegen. Bewerkt de `Name` veld met het nieuwe protocol de gewenste naam.

Nu bewerken voor het gebruik van dit protocol, de `App.xaml.cs` met de `OnActivated` methode, en vergeet niet initialiseren engagement hier ook:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 

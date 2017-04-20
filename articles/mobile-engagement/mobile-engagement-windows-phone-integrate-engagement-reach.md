<properties 
    pageTitle="Windows Phone Reach Silverlight SDK integratie" 
    description="Azure mobiele Engagement Reach integreren met Windows Phone Silverlight Apps"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Reach Silverlight SDK integratie

U moet de integratie-procedure in de [Integratie van Windows Phone Silverlight Engagement SDK](mobile-engagement-windows-phone-integrate-engagement.md) voordat u deze handleiding volgen.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>De SDK voor het bereiken van betrokkenheid insluiten in uw Windows Phone Silverlight-project

U hoeft niet iets toe te voegen. `EngagementReach`verwijzingen en bronnen zijn al in uw project.

> [AZURE.TIP]  U kunt afbeeldingen uit de `Resources` map van uw project, met name het merk-pictogram (dat standaard het pictogram betrokkenheid).

##<a name="add-the-capabilities"></a>Voeg de functies

De SDK Engagement bereiken moet enkele extra mogelijkheden.

Open de `WMAppManifest.xml` -bestand en zorg ervoor dat de volgende mogelijkheden worden aangegeven:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

De eerste die wordt gebruikt door de service MPNS de weergave van toast kennisgeving. In het tweede voorbeeld wordt gebruikt voor het insluiten van een taak van de browser in de SDK.

Bewerkt de `WMAppManifest.xml` -bestand en toevoegen in de `<Capabilities />` code:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>De Service Microsoft Push meldingen inschakelen

De **Push Notification Service van Microsoft** (MPNS genoemd) wilt gebruiken de `WMAppManifest.xml` bestand moet hebben een `<App />` label met een `Publisher` kenmerk ingesteld op de naam van het project.

##<a name="initialize-the-engagement-reach-sdk"></a>Initialiseren van de betrokkenheid bij de Reach-SDK

### <a name="engagement-configuration"></a>Betrokkenheid bij de configuratie

De betrokkenheid bij de configuratie is gecentraliseerd het `Resources\EngagementConfiguration.xml` -bestand van uw project.

Dit bestand bewerken als reach-configuratie:

-   *Optioneel*, geef aan of de native push (MPNS) wordt geactiveerd of niet tussen `<enableNativePush>` en `</enableNativePush>` -codes (`true` standaard).
-   *Optioneel*, de naam van de push-kanaal tussen `<channelName>` en `</channelName>` -tags, bieden dezelfde waarmee uw toepassing kan op dit moment of leeg laten.

Als u wilt dat in plaats daarvan tijdens de uitvoering te geven, kunt u de volgende methode toepassen voordat de agent betrokkenheid bij de initialisatie bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] U kunt de naam van het kanaal MPNS push van uw toepassing. Betrokkenheid wordt standaard een naam op basis van de toepassings-id gemaakt. U hebben hoeft de naam zelf opgeven, behalve als u van plan bent op het kanaal push buiten betrokkenheid.

### <a name="engagement-initialization"></a>Betrokkenheid bij de initialisatie

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Voeg `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Voeg `EngagementReach.Instance.OnActivated` in de `Application_Activated` methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] De `EngagementReach.Instance.Init` in een speciale thread wordt uitgevoerd. U hoeft niet zelf doen.

##<a name="app-store-submission-considerations"></a>App store indiening overwegingen

Microsoft legt enkele regels wanneer u de push-meldingen:

Punt 2.9 uit de [Beleidsregels voor toepassingen] van Microsoft-documentatie:

1) Je moet accepteren om push-meldingen ontvangen van de gebruiker vragen. Vervolgens voegt u in uw instellingen, kunt u de push-meldingen uitschakelen.

Het object EngagementReach bevat twee methoden voor het beheer van de opt-in/opt-out, `EnableNativePush()` en `DisableNativePush()`. U zou bijvoorbeeld een optie in de instellingen met een in-/ uitschakelen of uitschakelen MPNS maken.

U kunt ook besluiten MPNS deactiveren via de betrokkenheid van configuratie\<windows phone-sdk-reach-configuratie\>.

> 2.9.1) de toepassing moet eerst beschrijven de meldingen moeten worden gegeven en **verkrijgen van de uitdrukkelijke toestemming van de gebruiker (opt-in)**en **moet een mechanisme waarmee de gebruiker, niet deelnemen kan aan het push-meldingen ontvangen**. Alle meldingen die via de Service Microsoft Push-melding moet overeenkomen met de beschrijving voor de gebruiker en moet voldoen aan alle van toepassing zijnde [Beleidsregels voor toepassingen]  [ Content Policies] en [Aanvullende eisen voor specifieke toepassingen].

2) Gebruik niet te veel push-meldingen. Betrokkenheid bij de verwerkt meldingen voor u.

> 2.9.2) de toepassing en het gebruik van Microsoft Push Notification Service moeten niet overmatig netwerkcapaciteit of bandbreedte van Microsoft Push Notification Service gebruiken of anders ten onrechte een Windows Phone of andere Microsoft-aanwijsapparaat of de service met buitensporige push-meldingen, zoals bepaald door Microsoft in redelijke naar eigen goeddunken te belasten en beschadigen of geen invloed op alle Microsoft-netwerken of servers of servers van derden of netwerken die zijn verbonden met Microsoft Push Notification Service.

3) Vertrouw niet op MPNS criticals informatie te verzenden. Betrokkenheid wordt MPNS, zodat deze regel ook voor de campagnes die zijn gemaakt binnen de betrokkenheid bij de front-end geldt.

> 2.9.3) de Microsoft Push Notification Service kan niet worden gebruikt voor het verzenden van meldingen die mission critical of anderszins van invloed kunnen zijn op aangelegenheden van leven of dood, inclusief en zonder beperking kritieke meldingen gerelateerd aan een voorwaarde of een medisch hulpmiddel. MICROSOFT WIJST UITDRUKKELIJK ELKE GARANTIE DAT HET GEBRUIK VAN DE MICROSOFT-PUSH NOTIFICATION SERVICE OF LEVERING VAN MICROSOFT PUSH NOTIFICATION SERVICEMELDINGEN WORDEN ONONDERBROKEN, ZONDER FOUTEN, OF ANDERS KAN PLAATSVINDEN OP BASIS VAN REAL-TIME GEGARANDEERD.

**We kunnen niet garanderen dat uw toepassing het validatieproces wordt geaccepteerd als u bieden geen ondersteuning voor deze aanbevelingen.**

##<a name="handle-data-push-optional"></a>Verwerken van gegevens push (optioneel)

Als u wilt dat uw toepassing kunnen dit gereedschap duwt Reach gegevens ontvangen, hebt u twee gebeurtenissen van de klasse EngagementReach implementeren:

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

##<a name="customize-ui-optional"></a>Aanpassen van de gebruikersinterface (optioneel)

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

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met het aangepaste object in uw `App.xaml.cs` klasse binnen de `Application_Launching` methode.

**Voorbeeld van Code:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Betrokkenheid bij de standaard in de eigen implementatie van `EngagementReachHandler`. U hoeft niet uw eigen maken en als u dit doet, hoeft u niet elke methode overschrijven. Het standaardgedrag is het basisobject betrokkenheid te selecteren.

### <a name="layouts"></a>Lay-outs

Standaard bereik ingesloten bronnen van het DLL-bestand gebruikt om de berichten en pagina's weer te geven.

U kunt echter uw eigen resources gebruiken om uw merk in deze onderdelen weer te geven.

U kunt overschrijven `EngagementReachHandler` methoden in de subklasse betrokkenheid van uw lay-outs te zien:

**Voorbeeld van Code:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] De `CreateNotification` methode retourneert null. Het bericht niet weergegeven en de campagne reach worden weggehaald.

Ter vereenvoudiging van de uitvoering van de lay-out, bieden we ook onze eigen xaml die als basis voor uw code dienen kan. Deze bevinden zich in het archief van de betrokkenheid bij de SDK (/ src/bereiken /).

> [AZURE.WARNING] De bronnen die we aanbieden zijn exact dezelfde degene die wij gebruiken. Dus als u wilt dat ze rechtstreeks wijzigen, vergeet dan niet de naamruimte en de naam te wijzigen.

### <a name="notification-position"></a>Positie van de melding

Standaard wordt een melding van een in-app onder links aan de zijkant van de toepassing weergegeven. U kunt dit gedrag wijzigen door het overschrijven van de `GetNotificationPosition` methode van de `EngagementReachHandler` object.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Op dit moment kunt u kiezen tussen de `BOTTOM` (standaard) en `TOP` posities.

### <a name="launch-message"></a>Bericht starten

Wanneer een gebruiker klikt op een kennisgeving van systeem (toast), betrokkenheid van de app start, de inhoud van de push-berichten te laden en weergeven van de pagina voor de desbetreffende campagne.

Er is een vertraging tussen het starten van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van het netwerk).

Om aan te geven voor de gebruiker iets wordt geladen, moet u zelf een visuele informatie, zoals een voortgangsbalk of een voortgangsindicator. Betrokkenheid kan niet zelf, maar een paar handlers biedt u verwerken.

Voor het implementeren van de callback, doen:

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

U kunt de retouraanroep instellen in uw `Application_Launching` methode van uw `App.xaml.cs` bestand, bij voorkeur vóór de `EngagementReach.Instance.Init()` call.

> [AZURE.TIP] Elke-handler wordt aangeroepen door de UI-Thread. U hoeft geen zorgen te maken wanneer u een MessageBox of iets UI gerelateerd.

[Toepassingenbeleid]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Aanvullende eisen voor de van specifieke toepassingstypen]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 

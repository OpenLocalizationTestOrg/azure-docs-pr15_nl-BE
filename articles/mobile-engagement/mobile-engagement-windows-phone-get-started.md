<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor Windows Phone Silverlight apps"
    description="Informatie over het gebruik van Azure Mobile Engagement met analytics en push-meldingen voor Silverlight voor Windows Phone apps."
    services="mobile-engagement"
    documentationCenter="windows"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Aan de slag met Azure Mobile Engagement voor Windows Phone Silverlight apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement te begrijpen van het gebruik van app en push-meldingen verzenden naar gesegmenteerde gebruikers van een Windows Phone Silverlight-toepassing gebruiken.
Deze zelfstudie toont de eenvoudige broadcast scenario met behulp van Mobile Engagement. In, maakt u een lege Windows Phone Silverlight app die verzamelt basisgegevens en Push Notification Service (MPNS) met push-meldingen ontvangt.

> [AZURE.NOTE] Als u Windows Phone 8.1 (niet-Silverlight), verwijzen naar de [universele Windows zelfstudie](mobile-engagement-windows-store-dotnet-get-started.md).

Deze zelfstudie is het volgende vereist:

+ Visual Studio 2013
+ [MicrosoftAzure.MobileEngagement] Nuget package

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).

##<a id="setup-azme"></a>Setup-Mobile Engagement voor uw Windows Phone-app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "basic integration", dat is de minimaal vereiste gegevens verzamelen en verzenden van een push-bericht. De volledige integratie van documentatie vindt u in de [integratie van Mobile Engagement Windows Phone SDK](mobile-engagement-windows-phone-sdk-overview.md)

We gaan een eenvoudige toepassing maken met Visual Studio om aan te tonen van de integratie.

###<a name="create-a-new-windows-phone-silverlight-project"></a>Maak een nieuw project voor Windows Phone Silverlight

De volgende stappen wordt uitgegaan van het gebruik van Visual Studio 2015 maar de stappen in eerdere versies van Visual Studio zijn. 

1. Start Visual Studio en selecteer **Nieuw Project**in het scherm **Start** .

2. Selecteer in het pop-upmenu, **Windows 8** -> **Windows Phone** -> **Leeg App (Windows Phone Silverlight)**. Vul de app **naam** en de **naam van de oplossing**en klik vervolgens op **OK**.

    ![][1]

3. U kunt **Windows Phone 8.0** of **Windows Phone 8.1**.

U hebt nu een nieuwe Windows Phone Silverlight app waarin de SDK Azure Mobile Engagement integreert gemaakt.

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinding maken met uw app in de backend Mobile Engagement

1. Installeer het pakket [MicrosoftAzure.MobileEngagement] nuget in uw project.

2. Open `WMAppManifest.xml` (onder de map Eigenschappen) en zorg ervoor dat de volgende worden aangegeven (deze toevoegen als ze niet) in de `<Capabilities />` code:

        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />

    ![][2]

3. Nu de verbindingstekenreeks die u eerder hebt gekopieerd voor uw betrokkenheid bij de mobiele app plakken en plak deze in de `Resources\EngagementConfiguration.xml` bestand tussen de `<connectionString>` en `</connectionString>` tags:

    ![][3]

4. In de `App.xaml.cs` bestand:

    een. Voeg toe de `using` instructie:

            using Microsoft.Azure.Engagement;

    b. Initialiseren van de SDK in de `Application_Launching` methode:

            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }

    c. Invoegen van de volgende handelingen uit in het `Application_Activated`:

            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

##<a id="monitor"></a>Realtime-controle inschakelen

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één scherm (activiteit) sturen op de backend Mobile Engagement.

1. Voeg in de MainPage.xaml.cs, de `using` instructie:

        using Microsoft.Azure.Engagement;

2. De basisklasse van **MainPage**, die vóór de **PhoneApplicationPage**, vervangen door **EngagementPage**.

        class MainPage : EngagementPage 
    
3. In de `MainPage.xml` bestand:

    een. Voeg toe aan de declaraties van naamruimten:

         xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

    b. Vervangen `phone:PhoneApplicationPage` in de naam van de XML-code met `engagement:EngagementPage`.

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

Mobile Engagement kunt u interactie en het bereiken van uw gebruikers met Push-meldingen en Messaging-app in de context van campagnes. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
In de volgende secties, stel uw app ontvangen.

###<a name="enable-your-app-to-receive-mpns-push-notifications"></a>Uw app MPNS Push-meldingen ontvangen inschakelen

Toevoegen van nieuwe mogelijkheden voor uw `WMAppManifest.xml` bestand:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

###<a name="initialize-the-reach-sdk"></a>Initialiseren van de REACH-SDK

1. In `App.xaml.cs`, call `EngagementReach.Instance.Init();` rechts na de initialisatie van de agent in de functie **Application_Launching** :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

2. In `App.xaml.cs`, call `EngagementReach.Instance.OnActivated(e);` rechts na de initialisatie van de agent in de functie **Application_Activated** :

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

U bent klaar. Nu controleren wij het u juist hebt riep uit deze eenvoudige integratie.

##<a id="send"></a>Een bericht verzenden naar uw app

[AZURE.INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

U ziet nu een melding op het apparaat dat wordt weergegeven als een bericht in app als de app anders als een toast bericht als volgt openen is: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png

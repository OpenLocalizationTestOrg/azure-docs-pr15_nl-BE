<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor de eenheid van het Android-implementatie"
    description="Informatie over het gebruik van Azure Mobile Engagement met Analytics en Push-meldingen voor de eenheid van de apps op iOS-apparaten implementeren."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Aan de slag met Azure Mobile Engagement voor de eenheid van het Android-implementatie

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Dit onderwerp wordt beschreven hoe Azure Mobile Engagement om te begrijpen van het gebruik van app en het push-meldingen te verzenden gesegmenteerd gebruikers van een eenheid van toepassing bij het implementeren van een Android-apparaat.
In deze zelfstudie wordt de klassieke Unity roller een bal zelfstudie als uitgangspunt. U moet de stappen in deze [zelfstudie](mobile-engagement-unity-roll-a-ball.md) voordat u verdergaat met de betrokkenheid van mobiele integratie die wij in de handleiding hieronder presenteren. 

Deze zelfstudie is het volgende vereist:

+ [Eenheid van de Editor](http://unity3d.com/get-unity)
+ [Betrokkenheid bij de mobiele eenheid SDK](https://aka.ms/azmeunitysdk)
+ Google Android SDK

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).

##<a id="setup-azme"></a>Mobiele betrokkenheid bij de instellingen voor uw Android app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

###<a name="import-the-unity-package"></a>De eenheid van het pakket importeren

1. De [betrokkenheid van de eenheid van het mobiele-pakket](https://aka.ms/azmeunitysdk) downloaden en opslaan op uw lokale computer. 

2. Ga naar **activa -> pakket importeren -> aangepaste pakket** en selecteert u het gedownloade pakket in de vorige stap. 

    ![][70] 

3. Zorg ervoor dat alle bestanden zijn geselecteerd en klik op de knop **importeren** . 

    ![][71] 

4. Nadat het importeren is voltooid, ziet u de SDK-bestanden geïmporteerd in uw project.  

    ![][72] 

###<a name="update-the-engagementconfiguration"></a>De EngagementConfiguration bijwerken

1. Open het scriptbestand **EngagementConfiguration** van de update map SDK en de **ANDROID\_verbinding\_STRING** met de verbindingstekenreeks die u eerder hebt aangeschaft bij de Azure portal.  

    ![][73]

2. Sla het bestand 

3. Uitvoeren van **File -> Engagement -> Android Manifest genereren**. Dit is de invoegtoepassing toegevoegd door de SDK van Mobile Engagement en erop te klikken worden de instellingen van uw project automatisch bijgewerkt. 

    ![][74]

> [AZURE.IMPORTANT] Zorg ervoor dat dit uitvoeren zodra u het **EngagementConfiguration** bestand anders dat uw wijzigingen niet worden weergegeven in de toepassing bijwerken. 

###<a name="configure-the-app-for-basic-tracking"></a>De app voor eenvoudige tracering configureren

1. Open het **PlayerController** script dat is gekoppeld aan het object Player bewerken. 

2. Voeg de volgende instructie:

        using Microsoft.Azure.Engagement.Unity;

3. Voeg de volgende aan de `Start()` methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementeren en uitvoeren van de app.
Zorg ervoor dat u beschikt over Android SDK is geïnstalleerd op uw computer voordat u de implementatie van deze eenheid van de app naar uw apparaat. 

1. Een Android-apparaat aansluit op uw computer. 

2. Openen van **Bestand -> instellingen voor het maken** 

    ![][40]

3. **Android** selecteert en klikt u vervolgens op **Switch Platform**

    ![][51]

    ![][52]

4. Klik op **instellingen van de speler** en een geldige bundel-id. 

    ![][53]

5. Ten slotte klikt u op het **Bouwen en uitvoeren**

    ![][54]

6. U wordt mogelijk gevraagd om de naam van een map voor het opslaan van het pakket met Android. 

7. Als alles prima gaat, het pakket wordt gedistribueerd naar het aangesloten apparaat en ziet u de eenheid van het spel op uw telefoon! 

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

###<a name="update-the-engagementconfiguration"></a>De EngagementConfiguration bijwerken

1. Open het scriptbestand **EngagementConfiguration** van de update map SDK en de **ANDROID\_GOOGLE\_nummer** met het **Projectnummer Google** u eerder hebt verkregen van de Google wolk Developer-portal. Dit is een tekenreeks waarde dus zorg ervoor dat tussen dubbele aanhalingstekens. 

    ![][75]

2. Sla het bestand. 

3. Uitvoeren van **File -> Engagement -> Android Manifest genereren**. Dit is de invoegtoepassing toegevoegd door de SDK van Mobile Engagement en erop te klikken worden de instellingen van uw project automatisch bijgewerkt. 

    ![][74]

###<a name="configure-the-app-to-receive-notifications"></a>De app voor het ontvangen van meldingen configureren

1. Open het **PlayerController** script dat is gekoppeld aan het object Player bewerken. 

2. Voeg de volgende aan de `Start()` methode

        EngagementReachAgent.Initialize();

3. De app wordt bijgewerkt, implementeren en uitvoeren van de app op een apparaat per onderstaande instructies. 

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png

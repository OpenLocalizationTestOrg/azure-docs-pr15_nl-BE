<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor de eenheid van het iOS-implementatie"
    description="Informatie over het gebruik van Azure Mobile Engagement met Analytics en Push-meldingen voor de eenheid van de apps op iOS-apparaten implementeren."
    services="mobile-engagement"
    documentationCenter="unity"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-unity-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Aan de slag met Azure Mobile Engagement voor de eenheid van het iOS-implementatie

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Dit onderwerp wordt beschreven hoe Azure Mobile Engagement om te begrijpen van het gebruik van app en het push-meldingen te verzenden gesegmenteerd gebruikers van een eenheid van toepassing bij het implementeren van een iOS-apparaat.
In deze zelfstudie wordt de klassieke Unity roller een bal zelfstudie als uitgangspunt. U moet de stappen in deze [zelfstudie](mobile-engagement-unity-roll-a-ball.md) voordat u verdergaat met de betrokkenheid van mobiele integratie die wij in de handleiding hieronder presenteren. 

Deze zelfstudie is het volgende vereist:

+ [Eenheid van de Editor](http://unity3d.com/get-unity)
+ [Betrokkenheid bij de mobiele eenheid SDK](https://aka.ms/azmeunitysdk)
+ XCode-Editor

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).

##<a id="setup-azme"></a>Betrokkenheid bij de mobiele installatie voor uw iOS-app

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

1. Open het scriptbestand **EngagementConfiguration** van de update map SDK en de **IOS\_verbinding\_STRING** met de verbindingstekenreeks die u eerder hebt aangeschaft bij de Azure portal.  

    ![][73]

2. Sla het bestand. 

###<a name="configure-the-app-for-basic-tracking"></a>De app voor eenvoudige tracering configureren

1. Open het **PlayerController** script dat is gekoppeld aan het object Player bewerken. 

2. Voeg de volgende instructie:

        using Microsoft.Azure.Engagement.Unity;

3. Voeg de volgende aan de `Start()` methode
    
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

###<a name="deploy-and-run-the-app"></a>Implementeren en uitvoeren van de app.

1. Een iOS-apparaat aansluit op uw computer. 

2. Openen van **Bestand -> instellingen voor het maken** 

    ![][40]

3. **IOS** selecteert en klikt u vervolgens op **Switch Platform**

    ![][41]

    ![][42]

4. Klik op **instellingen van de speler** en een geldige bundel-id. 

    ![][53]

5. Ten slotte klikt u op het **Bouwen en uitvoeren**

    ![][54]

6. U wordt mogelijk gevraagd om de naam van een map voor het opslaan van het iOS-pakket. 

    ![][43]

7. Als alles prima gaat, het project wordt gecompileerd en u moet openen op uw toepassing XCode. 

8. Zorg ervoor dat de **bundel-id** in het project juist is.  

    ![][75]

10. Nu de app in XCode uitgevoerd zodat het pakket wordt gedistribueerd naar het aangesloten apparaat en u de eenheid van het spel op uw telefoon ziet! 

##<a id="monitor"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-meldingen inschakelen en messaging-app

Mobile Engagement kunt u interactief werken met de gebruikers en bereiken met push-meldingen en berichten in het kader van de campagnes in app. In deze module wordt bereiken in de portal Mobile Engagement genoemd.
U hoeft geen extra instellingen in de app om meldingen te ontvangen en deze al is ingesteld voor het.

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png

<properties
    pageTitle="Aan de slag met Azure Mobile Engagement voor Web Apps | Microsoft Azure"
    description="Informatie over het gebruik van Azure Mobile Engagement met analytics en push-meldingen voor Web Apps."
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="js"
    ms.topic="hero-article"
    ms.date="06/01/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Aan de slag met Azure Mobile Engagement voor Web Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp wordt beschreven hoe u Azure Mobile Engagement te begrijpen van het gebruik van Web App gebruiken.

Deze zelfstudie is het volgende vereist:

+ Visual Studio 2015 of een andere editor van uw keuze
+ [Web SDK](http://aka.ms/P7b453) 

Deze SDK Web in voorvertoning en alleen Analytics ondersteunt op het moment dat en verzenden van browser of in-app push meldingen nog niet wordt ondersteund. 

> [AZURE.NOTE] Deze zelfstudie hebt u een actieve account Azure. Als u geen account hebt, kunt u een gratis proefperiode account in een paar minuten. Zie voor meer informatie, [Gratis proefperiode van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).

##<a name="setup-mobile-engagement-for-your-web-app"></a>Mobiele betrokkenheid bij de instellingen voor uw Web app

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Verbinding maken met uw app in de backend Mobile Engagement

Deze zelfstudie bevat een "eenvoudige integratie," dat is de minimale vereiste om gegevens te verzamelen.

Een eenvoudige webtoepassing maken we met Visual Studio om aan te tonen de integratie, maar u kunt de stappen met een webtoepassing die ook buiten Visual Studio gemaakt. 

###<a name="create-a-new-web-app"></a>Maak een nieuwe Web App

De volgende stappen wordt uitgegaan van het gebruik van Visual Studio 2015 maar de stappen in eerdere versies van Visual Studio zijn. 

1. Start Visual Studio en selecteer **Nieuw Project**in het scherm **Start** .

2. Selecteer in het pop-upmenu, **Web** -> **ASP.Net-webtoepassingen**. Vul de app **naam**, **locatie** en de **naam van de oplossing**, en klik vervolgens op **OK**.

3. Selecteer **lege** onder **ASP.Net 4.5 sjablonen** in de pop-up **een sjabloon selecteert** en op **OK**. 

U hebt nu een nieuw, leeg Web App project waarin de SDK Azure Mobile Engagement Web integreert gemaakt.

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinding maken met uw app Mobile Engagement backend

1. Maak een nieuwe map genaamd **javascript** in uw oplossing en voeg het Web SDK JS bestand **azure-engagement.js** in het. 

2. Een nieuw bestand met de naam **main.js** in deze map javascript met de volgende code toevoegen. Zorg ervoor dat de verbindingsreeks. Dit `azureEngagement` object wordt gebruikt voor toegang tot Web SDK methoden. 

        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };

    ![Visual Studio met js-bestanden][1]

##<a name="enable-real-time-monitoring"></a>Realtime-controle inschakelen

Gegevens verzenden en ervoor te zorgen dat de gebruikers actief zijn gestart, moet u ten minste één activiteit op de backend Mobile Engagement te verzenden. Een activiteit in de context van een web app is een webpagina. 

1. Een nieuwe pagina met de naam **home.html** in uw oplossing maken en deze instellen als de startpagina voor uw web app. 
2. Zijn de twee javascripts die we eerder in deze pagina toegevoegd door het volgende in de body-tag toe te voegen. 

        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>

3. Bijwerken van de body-tag als u wilt aanroepen van EngagementAgent `startActivity` methode
        
        <body onload="engagement.agent.startActivity('Home')">

4. Hier is uw **home.html** zoeken
        
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

##<a name="connect-app-with-real-time-monitoring"></a>App verbinding met de realtime-controle

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

![][2]

##<a name="extend-analytics"></a>Analytics uitbreiden

Hier vindt u alle methoden die momenteel met Web SDK die u voor analytics gebruiken kunt:

1. Activiteiten/webpagina's:

        engagement.agent.startActivity(name);
        engagement.agent.endActivity();

2. Gebeurtenissen
        
        engagement.agent.sendEvent(name, extras);

3. Fouten

        engagement.agent.sendError(name, extras);

4. Taken

        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png


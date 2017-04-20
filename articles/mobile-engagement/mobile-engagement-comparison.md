<properties
    pageTitle="Azure Mobile Engagement vergelijken met andere vergelijkbare Azure services"
    description="Azure Mobile Engagement vergelijken met andere vergelijkbare Azure services - HockeyApp, AppInsights, melding Hubs"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure Mobile Engagement vergelijken met andere vergelijkbare Azure services

De lijst van diensten die worden aangeboden door Microsoft Azure is ooit uit te breiden en soms u vraagt zich misschien af hoe verschilt van Azure Mobile Engagement deze service die u zojuist hebt gelezen of gehoord over. Dit artikel probeert te verwijderen van de verwarring en zorgt ervoor dat u Azure Mobile Engagement kiezen wanneer is het meest geschikt is voor uw gebruik. 
 
Azure Mobile Engagement is een service speciaal **voor digitale marketing/CMOs** gericht, maar kan worden gebruikt door **mobiele app eigenaar of uitgever** die wil verhogen van het gebruik, de handhaving en monetization van hun mobiele apps. 

*Het is een software as a service (SaaS) gebruiker engagement platform dat biedt inzicht in het gebruik van app, real-time gebruiker segmentatie, gegevensgestuurde en contextueel bewust push-meldingen en messaging-app.* 

Deze definitie splitsen, hebben we de volgende essentiële kenmerken wat ook op de toegevoegde unieke waarde:

1.  **Contextueel bewust push-meldingen en messaging-app**
        
    Dit is de core focus van het product - gerichte en gepersonaliseerde push-meldingen uitvoeren. En om dit te gebeuren, verzamelen we de opmaak doorgevoerd analytics-gegevens. 

2.  **Gegevensgestuurde inzichten in het gebruik van app**

    Wij bieden een cross-platform SDK's voor het verzamelen van de analytics doorgevoerd over de app-gebruikers. Opmerking de term doorgevoerd analytics (tegenover prestaties analytics) omdat wij onze aandacht richten op hoe de app-gebruikers de app gebruiken. We verzamelen basisprestaties analytics-gegevens over fouten, vastlopen enz maar dat is niet de core focus van het product. 

3.  **Real-time gebruiker segmentatie**

    Zodra u app-gebruikers doorgevoerd analytics-gegevens hebt verzameld, kunt u op segment van uw doelgroep op basis van verschillende parameters en de verzamelde gegevens kunt u push gerichte campagnes worden uitgevoerd. 

4.  **Software-as-a-service (SaaS):**

    We hebben een apart van de Azure management portal die is geoptimaliseerd om te communiceren en uitgebreide doorgevoerd analyse over de app gebruikers weergeven en push marketingcampagnes voeren portal. Het product is gericht om u in geen tijd!   
 
Met deze set van differentiatie in de hand, hier is hoe we met andere schijnbaar vergelijkbaar Azure aanbiedingen vergelijken - aanpak om te bepalen welk product het beste werkt op basis van de opmerking dat het artikel geen vergelijking functie niveau maar een meer scenario heeft:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) is een van de Microsoft-oplossing voor mobiele DevOps. U kunt distribueren builds naar bèta-testers crash gegevens verzamelen en krijgt feedback van gebruikers. Ook worden geïntegreerd in de Visual Studio Team Services eenvoudig build implementaties en werk item integratie inschakelen. 
    
    De nadruk hier ligt op DevOps en verzamelen van prestaties analytics gegevens over de mobiele apps. Mogelijk dat u eindigt met het integreren van beide HockeyApps en Mobile Engagement in uw app en dat is niet ongebruikelijk omdat, hoewel er enige overlapping in de verzamelde gegevens is, de core focus van de producten verschilt, en ze helpen bij het bereiken van verschillende doelstellingen voor u.  

2.  [Inzichten van toepassing](../application-insights/app-insights-overview.md) Als uw mobiele app een server-side heeft, vervolgens gebruikt u inzichten van toepassing voor het controleren van de kant van de web server van uw app, maar voor de client-kant mobiele toepassingen, moet u de HockeyApp. 

3.  [Melding Hubs](https://azure.microsoft.com/services/notification-hubs/) biedt een lichtgewicht service in die zin dat u niet een SDK geïntegreerd in de mobiele app hoeft en er volledige controle over uw mobiele app en is het mogelijk met labels basismogelijkheden push-meldingen te verzenden. Dit is ideaal voor de eigenaar van een app die minder over gericht en meer informatie direct aan hun app-gebruikers (op een grote populatie broadcast) verzenden/communicatie is belangrijk. 

    Opmerking de focus op snelle meldingen met mogelijkheid basisindeling razendsnelle verzenden. 

Laten we enkele scenario's:

1.  TIM is onderdeel van de digitale marketingteam bij Adventure Works winkel die mobiele apps voor gebruikers wordt gepubliceerd. TIM van doel is om ervoor te zorgen dat de gebruikers die hun mobiele apps downloaden blijven gebruiken en regelmatig. Dit verhoogt niet alleen een merk koppelen met app-gebruikers, maar ook monetization toeneemt wanneer de gebruikers app aankopen via de mobiele app. Tim moet voor deze gerichte om meldingen te verzenden naar de app-gebruikers die ze nuttig vindt, aan te moedigen om de app te openen en Ga terug naar het vaak. Dit is waar Tim nuttig Mobile Engagement. 

2.  Joann maakt deel uit van het ontwikkelteam van de mobiele apps bij Adventure Works en is op zoek naar een product aan te melden gegevens over crashes, uitzonderingen en telemetrie prestaties u vanuit een app. Dit is waar Joann nuttig HockeyApp. Joann kan zowel HockeyApp voor haar ontwikkelaars gericht scenario's en Azure Mobile Engagement voor Tim integreren in de dezelfde app aan het beste van beide. 

3.  Robin is onderdeel van het ontwikkelteam van de mobiele apps bij Contoso nieuws en hebben alle die ze wil verzenden nieuwsberichten verbreken voor alle gebruikers zonder veel segmentatie als waarschuwing het nieuws is. Dit is waar Robin handig kennisgeving Hubs. In dit scenario is het echter mogelijk dat als de mobiele app veroudert, er is een vereiste segmentatie van veel rijkere en meer informatie over het gedrag van de gebruiker van de app. Op dit moment moet Robin evalueren Azure Mobile Engagement. 
 
Om recap, het doel van Mobile Engagement is niet alleen voor het verzamelen van analytics - het is niet 'nog een andere Analytics product van Microsoft'. Is over het verzenden van gerichte push-meldingen en voor dit doel instelt, verzamelen we doorgevoerd analytics-gegevens, maar blijft de focus op push-meldingen die de app gebruikers verwart zodat deze niet over als spam komt verzenden. 

Voor meer details - Kijk eens naar deze [korte video](mobile-engagement-overview.md) over Mobile Engagement kortom. 


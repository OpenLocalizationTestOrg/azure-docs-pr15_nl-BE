<properties 
   pageTitle="Azure mobiele betrokkenheid bij de Troubleshooting Guide - API 's" 
   description="Troubleshooting Guide voor mobiele Engagement Azure - API 's" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="10/04/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-api-issues"></a>Handleiding voor API-problemen oplossen

Hieronder vindt u mogelijke problemen die optreden kunnen met de wisselwerking tussen beheerders en Azure Mobile Engagement via de API's.

## <a name="syntax-issues"></a>Syntaxis van problemen

### <a name="issue"></a>Probleem
- Fouten in de syntaxis met de API (onverwachte).

### <a name="causes"></a>Oorzaken

- Syntaxis van problemen:
    - Controleer de syntaxis van de specifieke API die u gebruikt om te bevestigen dat de optie beschikbaar is.
    - Verwar de API bereiken en de Push API (de meeste taken moeten worden uitgevoerd met de API bereiken in plaats van de Push API) is een veelvoorkomend probleem bij gebruik van de API. 
    - Een ander veelvoorkomend probleem met de SDK-integratie en het gebruik van de API is verward de SDK en de API-sleutel.
    - Scripts die verbinding met de API's maken voor het verzenden van gegevens ten minste 10 minuten nodig of de verbinding wordt een time-out (met name voor Monitor API scripts gegevens). Als u wilt voorkomen dat de time-outs, hebben uw script een ping XMPP elke 10 minuten om de sessie met de server keepalive verzenden.

### <a name="see-also"></a>Zie ook
 
- [API-documentatie][Link 4]
- [XMPP Protocol Info]( http://xmpp.org/extensions/xep-0199.html)
 
## <a name="unable-to-use-the-api-to-perform-the-same-action-available-in-the-azure-mobile-engagement-ui"></a>Kan niet met behulp van de API beschikbaar is in de gebruikersinterface van Azure Mobile Engagement dezelfde actie uitvoeren

### <a name="issue"></a>Probleem
- Een actie die vanuit de gebruikersinterface van Azure Mobile Engagement werkt werkt van de verwante Azure Mobile Engagement API niet.

### <a name="causes"></a>Oorzaken

- Kunt u dezelfde actie uitvoeren vanuit de gebruikersinterface van Azure Mobile Engagement bevestigd wordt dat u deze functie van Azure Mobile Engagement correct hebt geïntegreerd met de SDK.

### <a name="see-also"></a>Zie ook
 
- [UI-documentatie][Link 1]
 
## <a name="error-messages"></a>Foutberichten

### <a name="issue"></a>Probleem
- Foutcodes weergegeven in Logboeken of runtime-API gebruiken.

### <a name="causes"></a>Oorzaken

- Hier wordt een lijst samengesteld van gemeenschappelijke API status codes getallen voor verwijzing en voorlopige probleemoplossing:

        200        Success.
        200        Account updated: device registered, associated, updated, or removed from the current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in the response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). The parameters provided to the API or service are invalid. In this case, the HTTP response will embed more details. Make sure to test for the MIME type of the response as the payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check the AppID and SDK key).
        402        Billing lock. The application is either off its quotas or is currently in a bad billing state.
        403        The application is not enabled or the specific API is disabled for this application.
        403        Unauthorized access to the project or application, invalid access key (the key must match the one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), the suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and the campaign’s property named, manual Push must be set to true.
        403        The email address is already associated to another account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        The email address is not associated with an account. Please create or update the account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying to edit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for the first time.
        409        Name already associated to a different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or the period is too large to be displayed (the server didn’t retrieve the analytics because the user request is for a period that is too large).
        503        Analytics not available yet (the requested information is not computed yet for an application).
        504        The server was not able to handle your request in a reasonable time (if you make multiple calls to an API very quickly, try to make one call at a time and spread the calls out over time).

### <a name="see-also"></a>Zie ook

- [API - documentatie van voor gedetailleerde fouten op elke specifieke API][Link 4]
 
## <a name="silent-failures"></a>Silent storingen

### <a name="issue"></a>Probleem
- Er wordt geen foutbericht weergegeven tijdens de uitvoering of in Logboeken API actie niet uitgevoerd.

### <a name="causes"></a>Oorzaken

- Veel items wordt in de gebruikersinterface van Azure Mobile Engagement wordt uitgeschakeld als deze zijn niet goed geïntegreerd maar zonder melding mislukken van de API, dus vergeet niet om te testen dezelfde functies uit de gebruikersinterface te zien als het werkt.
- Azure Mobile Engagement en veel geavanceerde functies van Azure Mobile Engagement die u probeert te gebruiken, moeten afzonderlijk geïntegreerd worden in uw app met de SDK als afzonderlijke stappen voordat u ze kunt gebruiken.

### <a name="see-also"></a>Zie ook

- [Troubleshooting Guide - SDK][Link 25]
 
<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 

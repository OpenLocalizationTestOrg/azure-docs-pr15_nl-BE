<properties 
   pageTitle="Azure mobiele betrokkenheid bij de Troubleshooting Guide - SDK" 
   description="Problemen met SDK integratie in Azure Mobile Engagement" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Problemen oplossen met integratieproblemen SDK

Hieronder vindt u mogelijke problemen optreden bij het Azure Mobile Engagement in uw toepassing integreert.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK-problemen ontdekt door een fout in een ander gebied van de toepassing

### <a name="issue"></a>Probleem
- Data collectie fout UI (in Analytics, Monitoring, segmentatie of Dashboards).
- Push-fouten (dit gereedschap duwt werken niet in app app of beide).
- Geavanceerde functie storingen (bijhouden, Geolocation of platform die specifieke gereedschap duwt werken niet).
- API-storingen (API's mislukken vaak stil zonder foutberichten).
- Service storingen (geen van Azure Mobile Engagement werkt voor uw toepassing).

### <a name="causes"></a>Oorzaken

- De meeste problemen die moeten worden opgelost met de SDK Azure Mobile Engagement moeten worden gevonden door een storing in uw toepassing (zoals een storing UI data collectie, push storingen, storing van de functie voor geavanceerde, API mislukte, toepassing loopt vast of zichtbare servicestoring).  
- Als een bepaalde functie van Azure Mobile Engagement nooit in uw app voor heeft gewerkt, moet u de integratie te voltooien. 
- Als een bepaalde functie van Azure Mobile Engagement is gestopt, moet u mogelijk een upgrade uitvoeren naar de laatste versie met de SDK Azure Mobile Engagement. Vergeet niet dat er een andere versie van de SDK Azure Mobile Engagement voor elk platform ondersteund door Azure Mobile Engagement (Android, iOS, Windows en Windows Phone).

#### <a name="sdk-integration"></a>SDK-integratie

- Azure Mobile Engagement wordt niet goed geïntegreerd in de SDK (Analytics).
- Bereiken niet goed geïntegreerd in de SDK (In App en van App duwt).
- Het certificaat is verlopen of onjuiste Prod.-order versus DEV (alleen iOS).
- GCM- of ADM niet goed geïntegreerd in de SDK (Android alleen - Service bepaalde duwt).
- Tracering niet goed geïntegreerd in de SDK (installeren archief bijhouden).
- Lazy locatie of GPS-locatie niet goed geïntegreerd in de SDK (doelitems op geo-locatie).


**Zie ook:**

- [SDK-documentatie - integratie hulplijnen][Link 5] 
- [Troubleshooting Guide - Push][Link 23]

#### <a name="sdk-upgrade"></a>SDK-Upgrade

- Upgrade nodig van de SDK voor het oplossen van problemen met oudere versies van de SDK (vaak in verband met nieuwere versies van het besturingssysteem van het apparaat).
- Alle vorige versies van uw app van het apparaat verwijderen en opnieuw installeren van de nieuwste versie van uw app, het opnieuw registreren uw apparaat-ID uit de gebruikersinterface Azure Mobile Engagement om te bevestigen dat uw apparaat met behulp van de nieuwste versie van uw app.

**Zie ook:**

- [SDK-documentatie - Release-opmerkingen](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK-documentatie - Upgrade hulplijnen](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Andere SDK

- Fouten in het Manifest van de toepassing "AndroidManifest.xml" kunnen leiden tot Azure Mobile Engagement niet te werken (alleen Android).
- Verwar de SDK en de API-sleutel is een veelvoorkomend probleem met de SDK-integratie en het gebruik van de API.

**Zie ook:**

- [Concepten - verklarende woordenlijst][Link 6]

## <a name="advanced-coding-issues"></a>Geavanceerde codering problemen

### <a name="issue"></a>Probleem
-  Platform-specifieke code niet rechtstreeks verband houden met Azure Mobile Engagement kan problemen veroorzaken op iOS, Android en Windows Phone.

### <a name="causes"></a>Oorzaken

- Veel geavanceerde codering problemen met Azure Mobile Engagement worden veroorzaakt door onjuist geschreven platform-specifieke code niet rechtstreeks verband houden met Azure Mobile Engagement. U moet in de documentatie specifiek aan het platform dat u naast Azure Mobile Engagement documentatie (Android, iOS, Web, Windows en Windows Phone ontwikkelt).
- 'Categorieën' niet correct configureert, kunnen koppelen vanuit een bericht naar een andere locatie binnen of buiten de toepassing (alleen Android). 
- Niet als "UIKit.framework" op uw iOS-code bevat een "Symbol niet gevonden fout" ' optional' en/of loopt vast op oudere iOS-apparaten (alleen iOS).
- Verlopen certificaten of push niet goed met de DEV of Prod.-versie van het certificaat, veroorzaakt problemen (alleen iOS).
- Er zijn enkele beperkingen die inherent zijn aan een platform dat Azure Mobile Engagement niet bepalen (zoals system center werking voor van app in Android en iOS duwt).
- Azure Mobile Engagement publiceert een volledige lijst van de interne pakketten Azure Mobile Engagement voor iOS en Android gebruikt ter referentie. Houd er rekening mee dat sommige functies van Azure Mobile Engagement specifiek aan het platform (Android, iOS, Web, Windows en Windows Phone zijn).

### <a name="see-also"></a>Zie ook

 - [Troubleshooting Guide - Push][Link 23] 
 - [SDK-documentatie - Release-opmerkingen][Link 5]
 - [SDK-documentatie - Upgrade hulplijnen][Link 5]

## <a name="application-crashes"></a>Vastlopende toepassingen

### <a name="issue"></a>Probleem
- De toepassing loopt vast op de eindgebruikers apparaat.

### <a name="causes"></a>Oorzaken

- Informatie over de crash kan worden weergegeven in de *Gebruikersinterface Analytics* of de *Analytics API*
- U kunt vinden de apparaat-ID van het testapparaat en de dezelfde actie uitvoeren die de toepassing voor een eindgebruiker om vast te stellen van de oorzaak van de crash crash veroorzaakt.
- Bekende problemen met de SDK Azure Mobile Engagement die ervoor zorgen dat loopt worden soms opgelost door te upgraden naar de nieuwste versie van de SDK. Controleer de release-opmerkingen over uw platform wanneer crashes te onderzoeken.

### <a name="see-also"></a>Zie ook

- [SDK-documentatie - Release-opmerkingen][Link 5]
- [SDK-documentatie - Upgrade hulplijnen][Link 5]

## <a name="app-store-upload-failures"></a>App store uploaden mislukt

### <a name="issue"></a>Probleem
- Fouten met betrekking tot de meest recente versie van uw app uploaden naar Apple, Google, of de Windows App store.

### <a name="causes"></a>Oorzaken

- App slaat soms apps blokkeren met bepaalde functies ingeschakeld (bijvoorbeeld de Apple-winkel wordt voorkomen dat het gebruik van IDFV in apps in de winkel en de winkel GooglePlay voorkomt dat de uitwisseling van toepassingsinformatie tussen apps). 
- Zorg ervoor dat u de release-informatie over het platform en de huidige versie van de SDK controleren als u problemen ondervindt bij het uploaden van een app in de winkel.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
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
 

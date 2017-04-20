<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - Reach hoe"
   description="Gebruikersinterface, overzicht voor betrokkenheid bij de mobiele Azure" 
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

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Gebruik en beheer van het werken duwt aan uw eindgebruikers bereiken

Zodra de SDK is volledig geïntegreerd in uw app, u kunt aan de slag met de de sectie bereik van de gebruikersinterface van Push-meldingen aan de gebruikers van uw app.  

## <a name="do-your-first-push-notification-campaign"></a>Uw eerste campagne voor Push-melding doen
-    Zorg ervoor dat uw bereik is geïntegreerd in uw app met de SDK. 
-    Selecteer uw toepassing
 
![First1][1]

-    Ga naar de sectie 'Reach' en klik op 'nieuwe aankondiging'
 
![First2][2]

-    Een nieuwe campagne maken en geef deze de naam
 
 ![First3][3]

-    Selecteer hoe de aanmelding moet worden geleverd als In-app alleen
 
![First4][4]

-    Het gewenste push-bericht maken
 
![First5][5]

-    U kunt een titel kan schrijven op de kennisgeving (optioneel).
-    Schrijf de inhoud van de push-bericht.
-    U kunt een afbeelding uploaden. Let erop dat de grootte van het bestand kan niet groter is dan 32.768 bytes.
-    Ook hebt u de mogelijkheid om nog meer opties kiezen, maar voor de focus van deze zelfstudie, we zien dat later.

-    Selecteer het inhoudstype alleen als aanmelding
 
![First6][6]

-    Maak uw campagne push en wordt weergegeven in de lijst van uw campagne.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Test uw campagne Push-meldingen
![Test1][8]

-    Registreer uw apparaat.
-    Schakel de selectievakjes in van het apparaat dat u wilt plaatsen.
-    Klik op de knop "Test" Druk op verzenden naar het apparaat.
 
![Test2][9]

-    De campagne activeren
 
![Test3][10]

-    Nu u uw campagne hebt gemaakt u gewoon moet activeren voor de kennisgeving moet worden gepusht naar uw gebruikers.
 
## <a name="send-personalized-pushes"></a>Dit gereedschap duwt persoonlijke verzenden
-    In dit voorbeeld wordt een push waar een aangepaste korting code is ingevoerd in de push-bericht.
 
![Personalize1][11]

Werken aan persoonlijke voorkeur aanpassen door een markering door uit een code app info dus vervangen, hebt u om te controleren of dat de gebruiker heeft de juiste app-info eerste gedefinieerd. In dit voorbeeld wordt de beoogde gebruikers een app info tag met de naam rebate_code gedefinieerd hebben.
Zoals u hierboven ziet bevat de inhoud push melding de markering ${rebate_code} die aan dat deze wordt vervangen door de inhoud van de code app info.

> Waarschuwing: Als u de app info-code is niet gedefinieerd voor de gebruiker, de gebruiker ontvangt de push.

-    Resultaat
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>U kunt de tekst verder personaliseren uw melding
![Personalize3][13]

-    De titel van het bericht, met inbegrip van de
-    En de inhoud van het bericht.
-    Kies het type van advertentie (tekst weergeven of de weergave)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>De hoofdtekst van een mededeling kan ook worden gepersonaliseerd met:
-    De actie-URL, moet u wilt de aanvoer pagina aanpassen
-    De titel,
-    De hoofdtekst van het bericht.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>De Push-meldingen te onderscheiden (in- of app)
-    Kies het type waarschuwing u push, selecteer uw toepassing, gaat u naar de sectie 'Bereiken', selecteer of een push campagne maken en Ga naar de sectie "Kennisgeving".
 
-    Klik op de modus"levering" die u wilt.
-    Klik op het selectievakje "Activiteiten beperken" Als u wilt dat de melding wordt uitgevoerd op specifieke activiteiten (schermen).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Bij App" leveringsmethode
![Differentiate2][16]

"Bij App" leveringsmethode biedt een push-bericht wanneer de toepassing wordt gesloten. Dit is de standaard push-bericht.
Als u 'van toepassing' selecteert, moet u al hebt verstrekt certificaten uit het platform dat u uw toepassing op (APNS of GCM ontwikkelt).

### <a name="see-also"></a>Zie ook
-  [Apple Push Notification Service – certificaten](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud Messaging-certificaat] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"in-App alleen" leveringsmethode
![Differentiate3][17]

Leveringsmethode "In-App alleen" biedt push-bericht wanneer de toepassing wordt uitgevoerd.
Voor deze mededeling hoeft u niet via het systeem voor APNS en GCM.
Het systeem voor verzending van in-app kunt u uw eindgebruikers bereikt.
U kunt volledig de melding en besluiten in welke activiteit (scherm) de melding wordt weergegeven.

### <a name="anytime-delivery-mode"></a>"Anytime" leveringsmethode
U kunt ervoor kiezen een leveringsmethode 'Altijd', zorgt ervoor dat u voor het bereiken van de eindgebruiker of de toepassing wordt uitgevoerd of niet.
Als u 'Altijd' selecteert, moet u al hebt verstrekt de certificaten van het platform dat u uw toepassing (APNS of GCM ontwikkelt). 
 
## <a name="schedule-a-push-campaign"></a>Een Push campagne plannen
### <a name="plan-to-start-a-campaign"></a>Plan een campagne starten
![Shedule1][18]

De 21e van maart is en u hebt een aankondiging te maken en voor de 22e maart om middernacht geschaafd. U hoeft niet te blijven voor de interface een push doen! Vooraf de exacte minuut meldingen worden verzonden, kunt u plannen.
-    UN-check de "geen" checkbox en selecteer een begintijd 
-    Kies de datum en het tijdstip op waarop u de push-campagne starten.

### <a name="plan-to-end-a-campaign"></a>Tot het einde van een campagne plannen
![Shedule2][19]

Uw campagne op de 25e maart stopt bij 3-d: 00 uur, maar u weet dat er niet meer om het te doen.
U hoeft niet te blijven voor de push-interface! U kunt vooraf de exacte minuut met uw campagne stopt plannen.
-    Klik op de "geen" checkbox of Selecteer een eindtijd
-    Kies de datum en de tijd die u wilt voltooien, de push-campagne.

### <a name="end-a-campaign-manually"></a>Een campagne handmatig beëindigen
![Shedule3][20]

Standaard wordt de 'geen'-selectievakjes zijn ingeschakeld.
Dit betekent dat de campagne wordt gestart zodra u in de sectie bereik activeren en eindigt wanneer u het in de sectie bereiken stopt.
 
> Opmerking: Campagnes gemaakt zonder einddatum de push lokaal opslaan op het apparaat en blijkt dat de volgende keer dat de toepassing wordt geopend, zelfs als de campagne is handmatig beëindigd.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Een Push-bericht met een tekstweergave verbeteren
### <a name="what-is-a-text-view"></a>Wat is een weergave van de tekst?
![TextView1][21]

Een tekstweergave is een pop-upvenster met de tekstinhoud. Dit pop-upmenu verschijnt als de eindgebruiker op een push-bericht.
Een tekstweergave kunt u meer inhoud aan uw eindgebruikers. Dit is ook de gelegenheid een oproep tot actie bijvoorbeeld springen naar een pagina van uw app, omleiden naar een winkel openen van een webpagina, een e-mail verzenden, starten van een zoekopdracht geo-gelokaliseerde enz...

### <a name="example-text-view"></a>Voorbeeld: Tekst weergeven
-    Uw campagne Push melding maken in de sectie 'Reach' en de campagne een naam geven
 
![TextView2][22]

-    Schrijf het bericht dat wordt weergegeven op de melding.
-    Selecteer het inhoudstype van de aankondiging van "tekst"
 
![TextView3][23]

> Opmerking: als u een tekstweergave drukt, altijd beschikt u over een bericht eerst. 

- De tekst definiëren (na hebt geselecteerd de tekstinhoud aankondiging de subsectie wordt weergegeven, zodat u de tekst die u wilt weergeven.)
 
![TextView4][24]

-    Schrijf de titel die boven aan het bericht wordt weergegeven.
-    De belangrijkste inhoud van de tekstweergave schrijven.
-    Schrijf de inhoud die wordt weergegeven op de actieknop (actieknop kan de toepassing om een bepaalde actie, zoals het openen van een pagina van de toepassing, omleiden naar een App-winkel of een type van bronnen die u kunt opgeven).
-    Schrijf de inhoud die wordt weergegeven op de knop Afsluiten (door te klikken op de knop Afsluiten, de tekstweergave verdwijnt.)
 
-    Uw campagne push melding maken en er op de lijst van de campagne.
 
![TextView5][25]

-    Activeer uw campagne push notification om de tekstweergave naar gebruikers verzenden.
 
![TextView6][26]

-    Resultaat
 
![TextView7][27]

-    De gebruiker krijgt de melding en klik erop.
-    De tekstweergave wordt weergegeven als een pop-upvenster zodat de gebruiker ermee.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Een Push-bericht met een webweergave verbeteren
### <a name="what-is-a-web-view"></a>Wat is de weergave van een webpagina
![WebView1][28]

Een webweergave is een pop-upvenster met webinhoud. Dit pop-upmenu verschijnt als de eindgebruiker op een push-bericht.
De weergave van een webpagina kunt u meer interactie met de eindgebruiker.
Dit is ook de gelegenheid een oproep tot actie, zoals omleiding naar App-winkel openen van een webpagina, een e-mailbericht verzenden, een geo-gelokaliseerde zoeken starten, enz...

### <a name="example-web-view"></a>Voorbeeld: Weergave op het Web
-    Een Push campagne maken in de sectie 'Reach' en de campagne een naam geven.
 
![WebView2][29]

-    Schrijf het bericht dat wordt weergegeven op de melding.
-    Selecteer het inhoudstype aankondiging als 'web'
 
![WebView3][30]

### <a name="about-announcement-types"></a>Aankondiging-typen:
- Kennisgeving: het is een eenvoudige standaard melding. Wat betekent dat als een gebruiker erop klikt, geen extra weergave wordt weergegeven, maar alleen de die gekoppeld zijn aan deze actie wordt uitgevoerd.
- Aankondiging van de tekst: het is een melding die voert de gebruiker een tekstweergave bekijken.
- Aankondiging web: Er is een melding die voert de gebruiker een weergave op het web bekijken.
Selecteer de inhoud "Aankondiging Web".

> Opmerking: Als u de weergave van een webpagina drukt, altijd beschikt u over een bericht eerst.

- Definieer de webinhoud (na hebt geselecteerd de webinhoud aankondiging de subsectie wordt weergegeven, zodat u voor het definiëren van de webinhoud moeten worden weergegeven.)

 
![WebView4][31]

-    Schrijf de titel die wordt weergegeven boven aan het bericht (optioneel).
-    Schrijf hier uw HTML-code.
-    Klik op de bron bewerken-modus om te schakelen edition en zien hoe het eruit ziet.
-    Schrijf de inhoud die wordt weergegeven op de actieknop (actieknop kan de toepassing om een bepaalde actie, zoals het openen van een pagina van de toepassing, omleiden naar een winkel of een type van bronnen die u kunt opgeven).
-    Schrijf de inhoud die wordt weergegeven op de knop Afsluiten (door te klikken op de knop Afsluiten, de weergave verdwijnt).
 
-    Resultaat
 
![WebView5][32]

-    De gebruiker ontvangt het bericht en klikt u op.
-    De tekstweergave wordt weergegeven als een pop-upvenster zodat de gebruiker ermee.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 

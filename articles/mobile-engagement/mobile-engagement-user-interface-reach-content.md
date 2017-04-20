<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - Reach-inhoud" 
   description="Informatie over het beheren van de unieke inhoud van de verschillende soorten push notification campagnes in Azure Mobile Engagement" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>De unieke inhoud van de verschillende soorten push notification campagnes beheren
 
U kunt de sectie inhoud van een nieuwe campagne van reach wijzigen van de inhoud van uw mededelingen, Polls, gegevens duwt en tegels (alleen Windows Phone). De instelling van de inhoud van de Push-campagnes is specifiek voor de campagne. 
 
### <a name="content-types"></a>Inhoudstypen:
- Aankondigingen
- Polls
- Dit gereedschap duwt gegevens
- Tegels (alleen Windows Phone)
 
## <a name="content-of-announcements"></a>De inhoud van aankondigingen
 ![Reach-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Kies het type van de aankondiging:
-    Kennisgeving: het is een eenvoudige standaard melding. Wat betekent dat als een gebruiker erop klikt, geen extra weergave wordt weergegeven, maar alleen de die gekoppeld zijn aan deze actie wordt uitgevoerd.
-    Aankondiging van de tekst: het is een melding die voert de gebruiker een tekstweergave bekijken.
-    Aankondiging web: Er is een melding die voert de gebruiker een weergave op het web bekijken.

### <a name="see-also"></a>Zie ook
- [Bereiken - hoe Tos - aankondigingen][Link 3] 

### <a name="about-web-view-announcements"></a>Over het Web weergeven aankondigingen:
Exemplaren van het patroon '{deviceid}' in de HTML-code of JavaScript-code die u hier opgeeft, wordt automatisch vervangen door de id van het apparaat dat de aankondiging wordt weergegeven. Dit is een eenvoudige manier om op te halen van de betrokkenheid van Azure mobiele apparaat-id's in een externe webservice gehost op uw back-office.
Als u wilt maken van een web-weergave Volledig scherm (zonder de standaard actie Afsluiten knoppen en wij bieden) kunt u de volgende functies vanuit JavaScript-code voor uw web view aankondiging: 

-    de actie van de aankondiging: ReachContent.actionContent()
-    afsluiten van de aankondiging: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Kies uw actie:

### <a name="about-action-urls"></a>Over de actie-URL's:
Een URL die kan worden geïnterpreteerd door een gerichte apparaat besturingssysteem kan worden gebruikt als een actie-URL.
Een speciale URL die uw toepassing kan ondersteunen (bijvoorbeeld om gebruikers naar een bepaald scherm gaan) kan ook worden gebruikt als een actie-URL.
Elk exemplaar van het patroon {deviceid} wordt automatisch vervangen door de id van het apparaat dat u de actie uitvoert. Dit kan gemakkelijk ophalen Azure betrokkenheid bij de mobiele apparaat-id's via een externe webservice in uw back-office worden gebruikt.

- **Android + iOS acties**
    - Een webpagina openen
    - http://\[web-site-domein\] 
    - Voorbeeld: http://www.azure.com
    - Een e-mailbericht verzenden
    - mailto:\[mail-ontvanger\]?subject =\[onderwerp\]& body =\[bericht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Een SMS-bericht verzenden
    - SMS:\[-telefoonnummer\] 
    - Voorbeeld: sms:2125551212
    - Een telefoonnummer kiezen
    - Tel:\[-telefoonnummer\] 
    - Voorbeeld: tel:2125551212
- **Android alleen acties**
    - Een toepassing op de Play Store downloaden
    - market://details?id=\[app pakket\] 
    - Voorbeeld: market://details?id=com.microsoft.office.word
    - Geo-gelokaliseerde zoeken
    - geo:0, 0?q =\[zoekopdracht\] 
    - Voorbeeld: geo:0, 0? q = starbucks, Parijs
- **iOS alleen acties**
    - Een toepassing op de App Store downloaden
    - http://iTunes.Apple.com/ [Land] /app/ [naam] /id [toepassing-id]? mt = 8 
    - Voorbeeld: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Acties voor Windows
    - Een webpagina openen
    - http://\[web-site-domein\] 
    - Voorbeeld: http://www.azure.com
    - Een e-mailbericht verzenden
    - mailto:\[mail-ontvanger\]?subject =\[onderwerp\]& body =\[bericht\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Verzenden van een SMS (Skype Store App vereist)
    - SMS:\[-telefoonnummer\] 
    - Voorbeeld: sms:2125551212
    - Een telefoonnummer (Skype Store App vereist)
    - Tel:\[-telefoonnummer\] 
    - Voorbeeld: tel:2125551212
    - Een toepassing op de Play Store downloaden
    - MS-windows-winkel: PDP?PFN =\[app pakket-ID\] 
    - Voorbeeld: ms-windows-winkel: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
    - Bingmaps zoeken
    - bingmaps:?q =\[zoekopdracht\] 
    - Voorbeeld: bingmaps:? q = starbucks, Parijs
    - Een aangepast schema gebruiken
    - \[aangepast schema\]://\[parameters aangepast schema\] 
    - Voorbeeld: myCustomProtocol://myCustomParams
    - Gebruik een pakketgegevens (Store App voor extensie lezen vereist)
    - \[map\]\[gegevens\]. \[uitbreiding\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Bouwt een URL bijhouden:
-    Zie de sectie 'Instellingen' in de <UI Documentation> voor instructies voor het bouwen van een tracking URL die gebruikers kunnen downloaden van een van de andere toepassingen zal leiden.
 
### <a name="define-the-texts-of-your-announcement"></a>De tekst van de aankondiging definiëren
Vul in de titel, de inhoud en de knop tekst van de aankondiging. U kunt een doelgroep van een toekomstige campagne op basis van de feedback bereiken van hoe gebruikers hebben gereageerd op de campagne op te richten. Doelgroepen kan worden gebaseerd op de feedback van u of deze campagne is net geduwd, beantwoord, mailbericht, of is afgesloten.

### <a name="see-also"></a>Zie ook
- [Nieuwe Push criterium van UI - Reach - documentatie][Link 28]

## <a name="content-of-polls"></a>Inhoud van de stemmingen
![Reach-Content2][31] vult u de titel, beschrijving en knop tekst van de aankondiging. Vervolgens voegt u vragen en opties voor antwoorden op uw vragen.
U kunt een doelgroep van een toekomstige campagne op basis van de feedback bereiken van hoe gebruikers hebben gereageerd op de campagne op te richten. Doelgroepen kan worden gebaseerd op of deze campagne is net geduwd, beantwoord, mailbericht, of is afgesloten. Doelgroepen kan ook worden gebaseerd op feedback van Poll antwoord, waar de vraag en antwoord keuze worden gebruikt als criteria.

### <a name="see-also"></a>Zie ook
- [Nieuwe Push criterium van UI - Reach - documentatie][Link 28]
 
## <a name="content-of-data-pushes"></a>De inhoud van de gegevens worden
![Reach-Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Kies het type van uw gegevens:
- Tekst
- Binaire gegevens
- Base64-gegevens

### <a name="define-the-content-of-your-data"></a>De inhoud van uw gegevens
- Als u tekstgegevens push, kopieer en plak de tekst in het vak 'inhoud'.
- Als u binaire of base64 push, gebruik de knop "uw bestand uploaden" om uw bestand te uploaden.
- U kunt een doelgroep van een toekomstige campagne op basis van de feedback bereiken van hoe gebruikers hebben gereageerd op de campagne op te richten. Doelgroepen kan worden gebaseerd op of deze campagne is net geduwd, beantwoord, mailbericht, of is afgesloten.

### <a name="see-also"></a>Zie ook
- [Nieuwe Push criterium van UI - Reach - documentatie][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Inhoud van de tegels (alleen Windows Phone)
![Reach-Content4][33]

### <a name="define-the-content-of-your-tile"></a>De inhoud van de tegel definiëren
De payload naast elkaar is de tekst moet worden weergegeven in het venster van uw app op Windows Phone-apparaten.
Een tegel push is de Push Notification Service (MPNS) versie van een native push voor Windows Phone. Het type tegel push is het enige type push waarop nog geen antwoord en dus de doelgroep van toekomstige campagnes van de resultaten van een tegel push campagne kan niet worden opgebouwd. 

### <a name="see-also"></a>Zie ook
- [Systeemeigen API - Reach-API - documentatie-Push][Link 4]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 

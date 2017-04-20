<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - bereik" 
   description="Meer informatie over het te bereiken voor de gebruikers van uw toepassing met Azure Mobile Engagement met push-meldingen" 
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


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Hoe te bereiken voor de gebruikers van uw toepassing met push-meldingen

Het tabblad **bereiken** van de portal **Mobile Engagement** beschreven. U kunt de portal **Mobile Engagement** bewaken en beheren van uw mobiele toepassingen. Houd er rekening mee dat als u wilt beginnen met de portal moet u eerst een **Azure Mobile Engagement** account maken. Zie [een Azure Mobile Engagement-account maken](mobile-engagement-create.md)voor meer informatie.

De sectie bereik van de gebruikersinterface is de Push campagne management tool u maken/bewerken/activeren/voltooien/monitor kunt en ontvang Push notification campagnes en functies die ook toegankelijk zijn via de API bereiken (en bepaalde elementen van de geringe Push API). Houd er rekening mee dat ongeacht of u de API's of de gebruikersinterface, u moet voor de integratie van zowel Azure Mobile Engagement en het bereik in uw toepassing voor elk platform met de SDK voordat u kunt bereiken campagnes.

>[AZURE.NOTE] Veel gedeelten van de portal **Mobile Engagement** UI bevatten de knop **HELP weergeven** . Druk op deze knop om meer contextuele informatie opvragen over een sectie.

## <a name="four-types-of-push-notifications"></a>Vier soorten Push-meldingen
1.    Aankondigingen - kunt u reclame berichten verzenden naar gebruikers die gebruikers naar een andere locatie in uw app omleiden of stuur ze naar een webpagina of een winkel buiten uw app. 
2.    Polls - kunt u om informatie te verzamelen van de eindgebruikers deze door vragen te stellen.
3.    Dit gereedschap duwt gegevens - kunt u een binair of een base64-bestand voor gegevens verzenden. De informatie in een push gegevens verzonden naar uw toepassing voor het wijzigen van uw huidige ervaring in uw app. De toepassing moet kunnen de gegevens in een push gegevens verwerken.

## <a name="campaign-details"></a>Campagnedetails

Bewerken, kopiëren, verwijderen of campagnes die zijn nog niet geactiveerd door de muis over de namen te activeren, of u kunt klikken op om deze te openen. U kunt campagnes die al zijn geactiveerd door de muis over de namen te klonen of klikt u op om deze te openen. Een campagne kunt u echter niet wijzigen nadat deze is geactiveerd.
 
![Reach1][18]

## <a name="reach-feedback"></a>Feedback bereiken

Klik op **Statistieken** kunt u de details van een campagne bereiken. De **eenvoudige** weergave biedt een visuele representatie in de vorm van een staafdiagram kolom over wat er is gebeurd na een campagne is geactiveerd. De weergave **Geavanceerd** biedt meer gedetailleerde informatie over de campagne push. Deze informatie is alleen beschikbaar als u een campagne test dat wil zeggen een push naar een testapparaat verzonden. Hier ziet u hoe deze gegevens moeten worden geïnterpreteerd:

1. **Pushed** - Hiermee geeft u het aantal berichten verplaatst naar de apparaten. Dit aantal is afhankelijk van de doelgroep die u hebt opgegeven tijdens het maken van de push-campagne. Als u geen waarde voor een doelgroep opgeeft, zal vervolgens deze push worden verzonden naar alle geregistreerde apparaten. Net als alle andere services push doen we push-meldingen niet rechtstreeks naar de apparaten maar ze in plaats daarvan push naar het desbetreffende platform specifieke Push Notification Services (PNS - GCM-APNS/WNS) zodat ze de kennisgevingen aan de apparaten bieden. 

2.  **Geleverd** - Hiermee geeft u het aantal berichten dat is geleverd door de PNS naar het apparaat en bevestigd als ontvangen door Mobile Engagement SDK. 
        
    *Redenen voor geleverd tellen lager is dan het aantal gedistribueerde:*
    
    1. Als de gebruiker heeft de app van het apparaat verwijderd, maar de PNS niet weten op het moment dat we de push naar de PNS verzenden wordt het bericht weggehaald.
    2. Als de apparaten zelf langere tijd offline was, maar het apparaat de app heeft, niet de PNS het bericht te bezorgen aan het apparaat. 
    3. Als het bericht naar het apparaat verzonden, maar de inhoud van het bericht niet wordt herkend door de SDK Mobile Engagement in de app, vervolgens verzonden het bericht. Dit kan gebeuren als de aanpassing van de melding op de app genereert een uitzondering die we werkelijk in de SDK en drop het bericht. Dit kan ook optreden als de app op het apparaat gebruikt een versie van de SDK van Mobile Engagement die niet weten wat de nieuwere versie van het push-bericht verzonden van het platform, maar dit is alleen mogelijk wanneer de app is bijgewerkt nadat het bericht is verzonden vanaf het service platform. Het tabblad **Geavanceerd** laat weten hoeveel berichten zijn verwijderd. 
    4. Op iOS-apparaten, worden berichten soms niet verzonden of het apparaat op accu of de app aanzienlijke hoeveelheid energie verbruikt bij de verwerking van externe meldingen. Dit is een beperking van de iOS-apparaten.   

3.  **Weergegeven** - Hiermee geeft u het aantal berichten die met succes worden weergegeven aan de gebruiker van de app op het apparaat in de vorm van een kennisgeving van de push/out-van-app systeem in het midden van de kennisgeving of een melding van een in-app in de mobiele app.  Het tabblad **Geavanceerd** ziet u hoeveel systeemmeldingen zijn en hoeveel in app-meldingen zijn. 
    
    *Redenen voor weergegeven aantal lager is dan het aantal van geleverd (in afwachting moeten worden weergegeven)*
    
    1. Als de melding campagne had een einddatum erop is het mogelijk dat het bericht is bezorgd, maar wanneer het tijd is geleverd om te openen en weer te geven voor de gebruiker van de app, het al verlopen is zodat deze nooit werd weergegeven.   
    2. Als de melding een melding van een in-app is wordt het bericht alleen weergegeven wordt wanneer de gebruiker app de app opent. In gevallen waarin de gebruiker app de app nog niet geopend, verslag de SDK dat de melding werd geleverd, maar nog niet weergegeven totdat de toepassing wordt geopend. 
    2. Als de melding een melding van een in-app is en geconfigureerd op een specifieke activiteit/scherm wordt weergegeven en vervolgens ook de kennisgeving worden vermeld als geleverd, maar nog niet is geleverd tot opent de gebruiker de toepassing op een bepaald scherm. 
    
4.  **Gebruikersinteracties** - Hiermee geeft u het aantal berichten die de gebruiker app heeft gecommuniceerd met de berichten die mailbericht of afgesloten zijn zijn. 

    - *De gebruiker van de app kan actie een melding in een van de volgende manieren:*
            
        1. Als de melding is een systeem/out-van-app bericht of een kennisgeving in app van verzonden bericht alleen-lezen en vervolgens de gebruiker app op de melding klikt.
        2. Als de melding een melding van een in-app met een tekst of webpagina weergeven of polls is vervolgens gebruiker de app op de actieknop in de kennisgeving.
        3. Als de melding een kennisgeving-app met een webweergave van is klikt de app gebruiker op een URL in de webweergave [alleen Android]
    
    - *De app-gebruiker kan een waarschuwing in een van de volgende manieren afsluiten:*
    
        1. Te klikken op de knop sluiten op de melding direct. 
        2. Weg te halen of te verwijderen van de kennisgeving. 
        3. Berichten met tekst/webinhoud en stemmingen in app worden meestal weergegeven aan de gebruiker app in een proces. Ze eerst een melding te zien en als ze op deze, zien ze de inhoud van de volgende web-tekst/poll. De gebruiker van de app een melding op een van de volgende stappen kunt afsluiten en de details in de weergave Geavanceerd wordt dit vastgelegd. 

5.  **Actioned** - Hiermee geeft u het aantal berichten die expliciet door de gebruiker app mailbericht waren. Dit is het nummer van de meest interessante weet hoeveel app gebruikers zijn geïnteresseerd door het bericht dat u in het bericht naar buiten wijzen. 
 
> [AZURE.NOTE] Op iOS & Windows platforms, als de gebruiker de app heeft openen en de campagne is een campagne "AnyTime" dan is het mogelijk dat beide app en in de app-meldingen worden weergegeven op hetzelfde moment. Hierdoor kan een aantal weergegeven hoger is dan de geleverd. Als de gebruiker communiceert of acties en zelfs het aantal interacties/Actioned van de gebruiker vervolgens de melding groter dan geleverd zijn kan. 


![Reach2][19]

## <a name="see-also"></a>Zie ook

- [Concepten][Link 6]
- [Troubleshooting Guide-Service][Link 24]

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
 

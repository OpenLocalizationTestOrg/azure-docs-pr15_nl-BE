<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - segmenten" 
   description="Meer informatie over het maken en beheren van segmenten van gebruikers met Azure Mobile Engagement gebruikspatronen te identificeren" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Het maken en beheren van segmenten van gebruikers gebruikspatronen te identificeren

Het tabblad **segmenten** van de portal **Mobile Engagement** beschreven. U kunt de portal **Mobile Engagement** bewaken en beheren van uw mobiele toepassingen.

De sectie segmenten van de gebruikersinterface kunt u werken aan uw gebruikers op basis van de ander gedrag en analytics dat u kunt ophalen van de toepassing en is ook toegankelijk via de API segmenten te segmenteren. Segmenten worden eerst 24 uur nadat ze zijn gemaakt en ze elke 24 uur op basis van de meest recente informatie voor analytics worden opnieuw berekend. Als een segment wordt berekend, wordt weergegeven een grafiek 'Dag aan dag geschiedenis' elke dag.


>[AZURE.NOTE] Veel gedeelten van de portal **Mobile Engagement** UI bevatten de knop **HELP weergeven** . Druk op deze knop om meer contextuele informatie opvragen over een sectie.

## <a name="create-segments"></a>Segmenten maken
U kunt een segment op basis van maximaal 10 criteria op een specifieke periode van 60 dagen in het verleden uit de sectie analytics maken. U kunt bijvoorbeeld een segment op basis van de mensen die bepaalde pagina's bekeken of gezocht naar specifieke inhoud in uw app in de afgelopen 10 dagen maken. Deze informatie is beschikbaar in de sectie analytics. Zo kunt u het maken van een segment en stel een push-bericht met als doel deze subset van gebruikers om ze terugkomen naar de toepassing. 
 
> Opmerking: Zodra een segment is berekend, deze kan niet worden bewerkt. alleen kan worden gekloond (kopiëren) of vernietigd (verwijderd). Een segment kan worden gekloond binnen dezelfde toepassing (met de AppID dezelfde) en kunnen ook worden gekloond in andere toepassingen (met een verschillende toepassings-id). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Segmenten voorbeelden
 ![segments2][36]

Segmenten kunnen u segmenten van de eindgebruikers van uw toepassing.
Verdeling van de gebruikers is een belangrijke marketingstrategie. Azure Mobile Engagement kunt u historische gegevens en aangepaste segmenten maken. Dit krachtige hulpprogramma kunt u meer informatie over de ervaring van uw klanten in uw toepassing. U kunt eenvoudig analyseren van de segmenten en de segmenten als push-doelen.
Een gemeenschappelijke gebruiksvoorbeeld is dat u wilt een push verzendt een bericht aan te moedigen de eindgebruikers van uw toepassing in de winkel te beoordelen. In plaats van een bericht te verzenden voor alle uw eindgebruikers, kunt u een segment dat alleen gebruikers die uw toepassing elke dag gebruikt voor de laatste maand en had een fantastische gebruikers wilt opgeven. Als u minder, zeer gerichte push-meldingen verzendt, krijgt u een beter Investeringsrendement.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Segmenten die kunt u op basis van de belangrijkste elementen die Azure Mobile Engagement:
- Gebeurtenis: een segment maken die doelen een specifieke gebeurtenis van de toepassing die meer dan twee keer per week is gebeurd. 
- Sessie: een segment van de gebruikers die de toepassing van de afgelopen week meer dan 5 keer maken.
- Activiteit: een segment van de gebruikers die op één pagina of inhoud meer of minder dan 10 keer afgelopen maand maken.
- Project: Maak een segment van de gebruikers die een taak meer dan twee keer per dag hebt voltooid.
- Crash: een segment van alle gebruikers waarvoor een crash vorige week meer dan 10 keer maken. (U kunt dit segment met een verontschuldiging of zelfs een coupon kan push!)
- Fout: een segment van alle gebruikers waarvoor een fout 3 dagen meer dan laatste 100 keer maken.
- App-Info: een segment die gericht is op een aangepaste App Info die er is gebeurd tijdens de laatste 25 dagen maken.
 
 ![segments4][38]

Voor het Segment optimaal gebruik, moet u hebben gedaan een aangepaste integratie van de SDK in uw toepassing met een plan tagging tags "App Info".
Ga naar de introductiepagina van de interface, selecteer de toepassing die u wilt gebruiken en klik op de sectie "Segmenten".

1. Selecteer in de sectie "Segmenten".
2. Klik op 'nieuw segment' knop om een nieuw segment te maken.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Real Life voorbeeld: Een eenvoudige segment op basis van een "Sessie" informatie maken
Maak een segment van de eindgebruikers die uw app ten minste 50 keer in de afgelopen week gebruikt. Daar vindt u alleen de eindgebruikers die ten minste 30 seconden in uw app per sessie hebt besteed. Hier ziet alle eindgebruikers die een positieve ervaring in uw app hebt. Het segment is gemaakt kan vervolgens worden gebruikt om een melding voor deze eindgebruikers om te vragen om te beoordelen van uw app in de winkel.
 
 ![segments5][39]

1. Geef uw segment een naam om snel te vinden in de lijst "Segment".
2. Klik op de knop 'Maken'.
 
 ![segments6][40]

Selecteer de sessie.
 
 ![segments7][41]

1. Selecteer de periode van 'De laatste week'.
2. Klik op volgende.
 
 ![segments8][42]

1. De Operator selecteren die relevant zijn uit de lijst: =; ≥, ≤.
2. Geef het gewenste aantal.
3. Het exemplaar dat u wilt selecteren. 
4. Klik op volgende.
In dit voorbeeld is ingesteld, zodat die in de laatste week overeenkomen met gebruikers die ten minste 50 sessies hebt aangebracht.
 
 ![segments9][43]

De sessie-segmentatie kunt u de lengte per sessie als criterium.

1. De Operator in de lijst selecteren.
2. Geef de lengte per sessie.
3. Klik op volgende.
In dit voorbeeld wordt aangegeven dat over alle sessies die hebben is gesegmenteerd in de sectie voorkomt, selecteert u alleen de gebruikers die meer dan 30 seconden per sessie hebt besteed.
 
 ![segments10][44]

Criterium te halen in de trechter van de volledige naam en klik op voltooien.
 
 ![segments11][45]

Wanneer u de criteria ingesteld hebt verschijnt in de trechter segment.
Omdat een segment op analytics-gegevens is gebaseerd, worden segmenten eenmaal per dag berekend.
In dit voorbeeld 47,7% van de totale eindgebruikers komt overeen met het criterium. Zij moeten de gebruikers die hebben een goede ervaring gehad en zal waarschijnlijk een hogere classificatie bieden als u ze een melding waarin u wordt gevraagd deze duwen te beoordelen van de app in de winkel.


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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 

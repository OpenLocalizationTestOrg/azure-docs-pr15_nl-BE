<properties
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - Analytics"
   description="Meer informatie over het analyseren van historische gegevens over uw toepassing met Azure Mobile Engagement"
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

# <a name="how-to-analyze-historical-data-about-your-application"></a>Analyse van historische gegevens over de toepassing

Het tabblad **ANALYTICS** van de portal **Mobile Engagement** beschreven. U kunt de portal **Mobile Engagement** bewaken en beheren van uw mobiele toepassingen. Houd er rekening mee dat als u wilt beginnen met de portal moet u eerst een **Azure Mobile Engagement** account maken.


De Analytics-sectie van de gebruikersinterface biedt geaggregeerde informatie over de toepassingen op basis van historische gegevens wordt elke 24 uur bijgewerkt. De informatie wordt weergegeven op andere dashboards bestaan uit staaf-lijn-cirkel-diagrammen, rasters en toewijzingen. De gegevens kunnen ook als .csv-bestanden worden gedownload. De meeste van deze informatie is beschikbaar in real-time in het gedeelte Beeldscherm van de gebruikersinterface en kan ook worden geopend vanuit de Analytics API.

>[AZURE.NOTE] Veel gedeelten van de portal **Mobile Engagement** UI bevatten de knop **HELP weergeven** . Druk op deze knop om meer contextuele informatie opvragen over een sectie.

## <a name="standard-and-custom-analytics"></a>Standaard- en aangepaste Analytics

Azure Mobile Engagement biedt een set van eenvoudige, standaard analytische informatie over de toepassingen die kunnen worden een diagram weergegeven aan als u uw toepassing met de SDK integreren. Azure Mobile Engagement biedt ook de mogelijkheid om extra aangepaste analytics informatie verzamelen die u over de werking van uw eindgebruikers. U kunt dit doen door het maken van een plan tag aangepaste "labels (app info)', gemaakt op basis van de **Instellingen** zodat Azure Mobile Engagement deze extra gegevens voor u verzamelen kunnen.



## <a name="analytics"></a>Analytics
- Toetsenbordpaneel: Bevat algemene informatie over de nieuwe en de desbetreffende gebruikers en hun ontwikkeling.
- Gebruikers: Gebruikers worden geïdentificeerd door hun apparaat-id: deze id is uniek voor elk apparaat (een nieuwe gebruiker is een nieuw apparaat). Een gebruiker wordt beschouwd als een nieuwe op een bepaalde tijdsinterval als hij zijn eerste zitting tijdens dit tijdsinterval is uitgevoerd. Een gebruiker wordt geacht worden bewaard als hij ten minste één sessie in de laatste 7 dagen heeft uitgevoerd. Actieve gebruikers zijn gebruikers die ten minste één sessie gedurende een bepaalde periode. U kunt sorteren, maandelijks, wekelijks, dagelijks of per uur perioden. Alle grafieken lijken, maar kunt u filteren op verschillende functies, zoals de versie van uw toepassing, en vervolgens om te sorteren op een bepaalde tijdsperiode. De standaardgegevens die zijn verzameld door de integratie van de SDK bevat de volgende: actieve gebruikers, nieuwe gebruiker, het aantal sessies, de lengte van elke sessie, technische informatie over land, lokale variabelen, locatie, taal vervoerder, apparaten, firmware, netwerk (Wi-Fi), versies van de SDK, app en die wordt gebruikt door klanten. Deze informatie kan worden bekeken in real-time uit de monitor sectie.

> Opmerking: De periode is gebaseerd op de datum van de instellingen voor de gebruikers, zodat een gebruiker wiens telefoon heeft de datum niet juist ingesteld kan in de verkeerde tijdsperiode.

- Bewaren: Een gebruiker wordt geacht worden bewaard op een bepaalde tijdsinterval als hij zijn eerste zitting tijdens dit tijdsinterval is uitgevoerd. Voor uren, dagen, weken of maanden kunt u de tijdsintervallen waarin ingehouden gebruikers (en nieuwe gebruikers) worden meegeteld. De gebruiker bewaren analytics is gebouwd op cohorten. Een cohort is de verzameling van alle nieuwe gebruikers voor een bepaalde periode wordt gedetecteerd (dat wil zeggen, de set met gebruikers die hun eerste sessie tijdens deze periode uitvoeren). We gebruiken de cohorten van 1 dag, 2 dagen, 4 dagen, 7 dagen of 1 maand. Aangezien een cohort, elke 1-dag 2-4 dagen, 7 dagen of 1 maand, Azure mobiele Engagement berekent de set van alle gebruikers die tot de cohort en er wordt behoren nog steeds actief (dat wil zeggen, de set met gebruikers die ten minste één sessie tijdens de periode wordt uitgevoerd). Deze groep gebruikers is een cohort versie genoemd. (Azure Mobile Engagement wordt weergegeven hoeveel gebruikers uw app nog steeds gebruikt, maar alleen de platform specifieke winkel weet u hoeveel gebruikers verwijderd uit de iTunes app - bijvoorbeeld GooglePlay, de Windows Store, enz.).
- Sessies: Één gebruik van de toepassing door een gebruiker. Sessies worden gegenereerd op basis van de volgorde van de activiteiten die worden uitgevoerd door gebruikers (een activiteit is meestal gekoppeld aan het gebruik van een scherm van de toepassing, maar dit kan variëren afhankelijk van de manier waarop de SDK is geïntegreerd in de toepassing). Een gebruiker kan slechts één activiteit uitvoeren op een tijdstip: een sessie wordt gestart zodra de gebruiker zijn eerste activiteit begint en stopt als hij zijn laatste activiteit is voltooid. Als een gebruiker meer dan een paar seconden blijft zonder uit te voeren activiteiten, is de volgorde van activiteiten opgedeeld in twee afzonderlijke sessies.
- Activiteiten: De namen van elk scherm in uw toepassing en de lengte van de gebruikers tijd besteden aan elk scherm. De activiteiten zijn een aangepaste analytische optie komt overeen met de "app info" codes die u hebt ingesteld voor uw eigen app:
- Pad naar gebruiker: Ziet u hoe gebruikers navigeren door de activiteiten van de toepassing (schermen). U kunt de schuifregelaar naar het niveau van details kunt verplaatsen. Blauwe knooppunten staan voor de activiteiten van uw toepassing. De grootte is evenredig aan de tijd die gebruikers die erin wordt besteed. Witte knooppunten vertegenwoordigen sessie starten en stoppen. Rode knooppunten vertegenwoordigen crashes. Koppelingen geven een overgang tussen de activiteiten van de toepassing (of activiteiten en loopt vast). Klik op een knooppunt of een koppeling om knopinfo met meer informatie over uw gegevens weer te geven: de tijd besteed aan een bepaald venster de telling van overgangen en het percentage van de overgangen van de activiteit van de bron naar de bestemming activiteit. (Een---60%---> B betekent dat gebruikers op een activiteit gaat naar activiteit B 60% van de tijd.) U kunt de grafiek opnieuw indelen als u nodig hebt om aan te geven de positie wordt opgeslagen telkens wanneer u een wijziging aanbrengt. U kunt weergeven of verbergen van het uitvallen van de computer om de grafiek lichter te maken.
- Gebeurtenissen: Specifieke acties van een gebruiker in de toepassing. De verdeling van de gebeurtenissen wordt weergegeven als het aantal gebeurtenissen per gebruiker, per sessie. Een gebeurtenis vertegenwoordigt een onmiddellijke actie, bijvoorbeeld een klik op een knop of de ontvangst van een kennisgeving. (De betekenis van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een gebeurtenis kan kan optreden tijdens een sessie of een taak of zelfstandig worden gebruikt.
- Taken: Vergelijkbaar met de gebeurtenissen, tenzij ze zich op de duur van de actie richten. Bijvoorbeeld kunnen taken vertellen u technische informatie over hoe lang het duurt inhoud te laden of een aanroep van een webservice voordat. Het kan ook aangeven hoe lang het duurt voordat een gebruiker een formulier invullen, maak een account of een aankoop te doen. Een taak de duur van een taak aangeeft, bijvoorbeeld de duur van een downloadtaak of de tijd een informatieregel wordt weergegeven op het scherm. (De betekenis van taken is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Taken zijn gewoonlijk gekoppeld aan achtergrondtaken die worden uitgevoerd buiten het bereik van een sessie (dat wil zeggen, zonder enige gebruikersactiviteit).
- Technicals: Technische informatie over de apparaten van de gebruikers van uw toepassing bijhouden, zoals landinstellingen, vervoerder, netwerk, apparaat, Firmware, en de grootte van de gebruikers, apparaten en de versie van uw App en de SDK-versie die wordt gebruikt in uw app scherm.
- Fouten: Informatie over technische fouten in de toepassing waardoor niet de toepassing vastloopt. Een fout vertegenwoordigt een instant probleem, zoals een netwerkfout of een slechte manipulatie. (De betekenis van gebeurtenissen is afhankelijk van hoe de SDK is geïntegreerd in de toepassing.) Een fout kan kan optreden tijdens een sessie of een taak of zelfstandig worden gebruikt.
- Crashes: Informatie over fouten die ertoe leiden de toepassing dat vastloopt. Een crash is een onverwachte gebeurtenis waarbij de toepassing stopt de verwachte functies uitvoeren en moet worden gestopt. Een crash wordt meestal veroorzaakt door een fout in de toepassing.

![Analytics2][11]

## <a name="accessing-the-retention-overview"></a>Toegang tot het overzicht bewaren
![Analytics3][12]

Het overzicht bewaren wordt opgesplitst in het midden in de verschillende kaarten, elk met het overzicht voor een bepaalde periode. De bewaartermijn van 2 dagen wordt in het voorbeeld weergegeven. De andere kaarten tonen de punten 4 en 7 dagen bewaren.

## <a name="understanding-the-retention-overview-cards"></a>Wat zijn de kaarten vasthouden, overzicht
![Analytics4][13]

### <a name="each-card-is-composed-of-3-main-parts"></a>Elke kaart bestaat uit 3 hoofdonderdelen:
1. 1: de cohort en de periode beschouwd als
2. 2-4: de inhouding voor de huidige periode
3. 5: een Sparkline van de geschiedenis

### <a name="here-is-detailed-information-about-each-element"></a>Hier volgt gedetailleerde informatie over elk element:
1.    Cohort en periode: deze kop geeft het type van de cohort. Hier houdt '2 dagen' in dat gaan we het gedrag van de gebruikers meer dan 2 dagen gebruikers dat is ontvangen in een periode van 2 dagen, en of zij een verbinding maken in de volgende blokken van 2 dagen. In het bovenstaande voorbeeld rekening gehouden met de activiteiten van gebruikers tussen de 21 en de 22e van November.
2.    Dit resulteert in de rentabiliteit van de inhouding via de 21 en 22 november voor de gebruikers die aankomen in 19 en 20 november. Hier hebben we 1 van de actieve gebruiker tussen de 21 en de 22e, via de 3 nieuwe gebruikers tussen de 19e en 20e waren.
3.    Deze visuele indicator geeft dezelfde informatie als bovenstaande grafisch weergegeven. (De derde van de cirkel is vanaf het nummer 33%). De kleur geeft extra informatie: groen geeft dit aantal groeit uit de vorige berekening. Geel betekent dat stabiel en rood betekent verlagen.
4.    Dit betekent dat de waarden die worden gebruikt voor de berekening.
5.    Dit is een Sparkline over de geschiedenis van de inhouding waarden. U kunt de waarden te vinden in het verleden hebben een breed beeld van hoe het zich ontwikkeld.


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

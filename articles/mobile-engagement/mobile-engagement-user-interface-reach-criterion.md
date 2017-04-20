<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - Reach-criterium" 
   description="Informatie over het gebruik van criteria gerichte campagnes push verzenden naar een specifieke subset van de gebruikers met behulp van Azure Mobile Engagement" 
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


# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a>Het gebruik van criteria gerichte campagnes push verzenden naar een specifieke subset van de gebruikers

Uw doelgroep gericht op specifieke criteria met de knop "Nieuwe Criteria" is een van de meest krachtige concepten in Azure Mobile Engagement dat helpt u relevante verzenden push-meldingen dat de klanten reageren zullen op in plaats van iedereen spam. U kunt uw doelgroep op basis van standaard criteria beperken en simuleren dit gereedschap duwt om te bepalen hoeveel mensen de melding ontvangt.

**Zie ook:**

- [UI - Reach - documentatie nieuwe Push campagne][Link 27]

## <a name="audience-criteria-can-include"></a>Criteria voor publiek kunnen opnemen:
- **Technicals:** U kunt richten op basis van dezelfde technische informatie u in de secties Analytics en Monitor bekijken kunt. **Zie ook:** [Documentatie voor UI - Analytics] [ Link 15], [Documentatie voor UI - Monitor][Link 16]
- **Locatie:** Toepassingen die gebruikmaken van 'Real time locatie rapporteren' met Geo hekwerk kunnen Geo-locatie gebruiken als een criterium te richten op een doelgroep van de GPS-locatie. "Lazy gebied locatie rapportage" oproep ook worden gebruikt voor het richten op een doelgroep van de locatie van de mobiele telefoon ('Real time locatie rapporteren' en 'Lazy gebied locatie rapporteren' moet worden geactiveerd vanuit de SDK). **Zie ook:** [SDK-documentatie - iOS - integratie] [ Link 5], [SDK-documentatie - Android - integratie][Link 5]
- **Feedback bereiken:** U kunt uw doelgroep op basis van de feedback van eerdere reach meldingen via reach feedback van aankondigingen, Polls en duwt gegevens kunt richten. U kunt beter doel uw publiek na twee of drie campagnes bereiken dan de eerste keer. Het kan ook worden gebruikt voor het filteren van gebruikers die al een melding met soortgelijke inhoud, ontvangen door een campagne niet worden verzonden naar gebruikers die een specifieke campagne in de vorige reeds ontvangen. U kunt ook voorkomen dat gebruikers een bepaalde campagne die is nog steeds actief nieuwe ladingen te ontvangen die zijn opgenomen. **Zie ook:** [Documentatie van de UI - bereiken - Push-inhoud][Link 29]
- **Installeren bijhouden:** Informatie op basis van waar uw gebruikers uw App geïnstalleerd, kunt u bijhouden. **Zie ook:** [Documentatie van gebruikersinterface - instellingen][Link 20]
- **Profiel:** U kunt richten op basis van standaard gebruikersgegevens en u kunt doel op basis van de gegevens van de aangepaste toepassing die u hebt gemaakt. Dit omvat de gebruikers die momenteel zijn aangemeld en gebruikers die specifieke vragen die u hebt gevraagd deze in te stellen in de app zelf in plaats van alleen hoe ze hebben gereageerd op campagnes vorige beantwoord. Alle van de App-Info voor uw app te zien op deze lijst hebt gedefinieerd.
- Segmenten: U kunt ook doel op basis van segmenten die u hebt gemaakt op basis van de specifieke gebruikersgedrag met meerdere criteria. Alle van de segmenten die zijn gedefinieerd voor uw app weergegeven in deze lijst. **Zie ook:** [Documentatie voor UI - segmenten][Link 18]
- **App Info:** "Instellingen" voor het bijhouden van gebruikersgedrag kunnen aangepaste App Info codes worden gemaakt. **Zie ook:** [Documentatie van gebruikersinterface - instellingen][Link 20]

## <a name="example"></a>Voorbeeld: 
Als u een aankondiging push alleen naar de deelpopulatie van de gebruikers die een actie in-app aankoop hebben uitgevoerd.

1. Ga naar de pagina-instellingen, selecteert u het menu "info App" en selecteer 'Nieuwe app info'
2. Registreer een nieuwe Boolean app info genaamd "inAppPurchase"
3. Uw toepassing dit app info op 'true' wanneer de gebruiker met succes een aankoop in de app uitvoert instellen (met behulp van de sendAppInfo ("inAppPurchase",...) functie)
4. Als u niet wilt dat om dit te doen vanuit uw toepassing, kunt u dit doen vanuit de back-end met behulp van de API-apparaat)
5. Alleen moet u uw aankondiging maken met een criterium te beperken uw publiek gebruikers met 'inAppPurchase' ingesteld op 'true')
 
> Opmerking: Gericht op basis van andere criteria dan app info tags vereist Azure Mobile Engagement om informatie te verzamelen van uw gebruikers apparaten voordat de push wordt verzonden en dus een vertraging kan veroorzaken. Configuratie van complexe push opties (zoals badges bijgewerkt) kunnen ook worden vertraagd duwt. Met behulp van een "één keer" campagne van de Push API is de absolute snelste pushmethode in Azure Mobile Engagement. Gebruik alleen app info codes als push-criteria voor een campagne bereiken (via de API bereiken of de gebruikersinterface) is de volgende snelste methode omdat app info tags worden opgeslagen op de server. Andere criteria voor gerichte is voor een push campagne de meest flexibele maar traagste pushmethode Azure Mobile Engagement heeft om de apparaten een query verzenden van de campagne.
 
![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a>Criterium-opties van toepassing op:
- **Technicals**     
- Naam van de firmware: naam van de Firmware
- Firmware-versie: Firmware-versie
- Apparaat model: apparaat model
- Fabrikant: fabrikant van het apparaat
- Toepassingsversie: versie van de toepassing
- Naam van de vervoerder: de naam van de vervoerder, undefined
- Vervoerder land: land vervoerder niet gedefinieerd
- Netwerktype: type netwerk
- Land: land
- Schermgrootte: grootte van het scherm
- **Locatie**      
- Laatst bekende gebied: land, regio, plaats
- Real time geo-hekwerk: lijst van bezienswaardigheden en nuttige locaties (naam, acties), ronde pijl (naam, Latitude, lengte, straal in meter)
- **Feedback bereiken**     
- Aankondiging-feedback: aankondiging, feedback
- Feedback poll: Poll, feedback
- Antwoord, feedback vragen: vragen antwoord, feedback, vraag, keuze
- Gegevens Push feedback: gegevens Push, feedback
- **Installeer bijhouden**     
- Archief: Archief, Undefined
- Bron: Bron, niet gedefinieerd
- **Gebruikersprofiel**     
- Geslacht: mannelijk of vrouwelijk, niet gedefinieerd
- Datum geboren: operator, datum, niet gedefinieerd
- Opt-in: true of false, niet gedefinieerd
- **App Info**      
- Tekenreeks: Ongedefinieerde String
- Datum: de operator, datum, niet-gedefinieerde
- Integer: exploitant, nummer, niet gedefinieerd
- Boole-waarde: waar of ONWAAR, niet gedefinieerd
- **Segment**    
- Naam van segmenten (vervolgkeuzelijst) uitsluiting (target-gebruikers die geen deel van dit segment uitmaken).

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
 

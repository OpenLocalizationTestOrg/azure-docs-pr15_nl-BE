<properties 
   pageTitle="Betrokkenheid bij de mobiele Azure User Interface - Reach-campagne" 
   description="Laern hoe campagnes maken en beheren van push-bericht met Azure Mobile Engagement" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Het maken en beheren van campagnes voor push-meldingen
Het gedeelte van het bereik van de gebruikersinterface kunt u een nieuwe Push campagne maken met een complexe formule door middel van de informatie die u nodig hebt voor het verzenden van een push-bericht. De opties van een Push campagne enigszins variëren afhankelijk van de vier campagnetypen: aankondigingen, Polls gegevens duwt en tegels (alleen Windows Phone).

### <a name="option-applies-to"></a>Optie is van toepassing op:
- Talen: Alle (aankondigingen, Polls, gegevens dit gereedschap duwt, tegels)
- Campagne: Alle (aankondigingen, Polls, gegevens dit gereedschap duwt, tegels)
- Melding: Aankondigingen, Polls
- Inhoud: Uniek voor elk campagnetype
- Doelgroep: Alle (aankondigingen, Polls, gegevens dit gereedschap duwt, tegels)
- Periode: aankondigingen, Polls, tegels
- Test: Alle (aankondigingen, Polls, gegevens dit gereedschap duwt, tegels)
 
![Reach-Campaign1][20]

## <a name="languages"></a>Talen
Het menu talen kunt u een andere versie van uw Push verzenden naar apparaten die zijn ingesteld op het gebruik van verschillende talen. Alle apparaten ontvangen standaard dezelfde druk ongeacht de taal waarin ze zijn ingesteld om te gebruiken. De versie van de standaardtaal van de Push ontvangen gebruikers met hun apparaat ingesteld op een andere taal. Veel van de opties voor push campagne kunnen u alternatieve inhoud opgeven voor elk van de andere talen die u selecteert. 
 
![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a>Verschillen in taal van toepassing op:
- Talen: Unieke talen kunnen worden geselecteerd naast de standaardtaal
- Campagne: Dezelfde voor alle talen
- Melding: Uniek voor elke taal naast de standaardtaal
- Inhoud: Uniek voor elke taal naast de standaardtaal
- Doelgroep: Kan worden gefilterd op een aparte taal criterium
- Periode: dezelfde voor alle talen
- Test: Voor elke taal kunnen worden verzonden per keer
 
### <a name="supported-languages"></a>Ondersteunde talen:
- Arabisch (ar) 
- Bulgaars (bg) 
- Catalaans (ca) 
- Chinees (zh) 
- Kroatisch (hr) 
- Czech (CS) 
- Deens (da) 
- Nederlands (nl) 
- Engels (en) 
- Finse (fi) 
- Frans (fr) 
- Duits (de) 
- Grieks (el) 
- Hebreeuws (hij) 
- Hindi (hi) 
- Hongaars (hu) 
- Indonesisch (id) 
- Italiaans (it) 
- Japans (ja) 
- Koreaans (ko) 
- Lets (lv) 
- Litouws (lt) 
- Maleis (macrolanguage) (ms) 
- Noors Bokmål (nb) 
- Pools (pl) 
- Portugees (pt) 
- Roemeens (ro) 
- Russisch (ru) 
- Servisch (sr) 
- Slowaaks (sk) 
- Sloveens (sl) 
- Spaans (es) 
- Zweeds (sv) 
- Tagalog (tl) 
- Thais (th) 
- Turks (tr) 
- Oekraïens (uk) 
- Vietnamees (vi) 
 
## <a name="campaign"></a>Campagne
De naam en de categorie van uw campagne ook instellen als u van plan bent het publiek gedeelte van een Push campagne negeren en verzenden deze campagne via de API bereiken (en sommige elementen met de geringe Push API) in plaats daarvan kunt u de campagne-sectie. Categorieën kunnen worden gebruikt met een aangepaste meldingssjabloon control-app meldingen op basis van vooraf gedefinieerde instellingen. U kunt een lijst van de bestaande "categorieën" via de API bereiken.

> Waarschuwing: Als u de optie 'Publiek negeren, push zal worden verzonden naar gebruikers via de API ' in de sectie "Campagne" van een campagne bereiken de campagne wordt niet automatisch verzenden, moet u handmatig verzenden via de API bereiken.
 
![Reach-Campaign3][22]
 
### <a name="option-applies-to"></a>Optie is van toepassing op:
- Naam: alle
- Categorie: Aankondigingen, Polls
- Negeren van de doelgroep, push zal worden verzonden naar gebruikers via de API: alle
 
## <a name="notification"></a>Kennisgeving
U kunt de sectie kennisgeving basisinstellingen instellen voor het opnemen van de push: de titel van de Push, het bericht, een afbeelding in-app, of als deze worden weggeklikt. Veel instellingen voor meldingen zijn specifiek aan het platform van het apparaat. Kunt u aangeven of de push 'in de app' of ' app' worden verzonden of beide. (Houd er rekening mee dat gebruikers kunnen 'opt-in' of 'opt out' van 'van toepassing' op het besturingssysteem niveau op hun apparaten duwt en Azure Mobile Engagement worden niet overschrijft deze instelling. Vergeet ook niet dat de API bereiken 'in app verwerkt' en 'onvoldoende app' duwt. De Push API kan worden gebruikt 'van toepassing' gereedschap duwt te verwerken.) Dit gereedschap duwt kunnen worden aangepast met afbeeldingen of HTML-inhoud, inclusief diepe links voor het koppelen van buiten uw App of naar een andere locatie in uw App (Android SDK 2.1.0 of hoger uitvoerintentie categorieën vereist). U kunt het pictogram of iOS badge wijzigen en tekst of webpagina-inhoud (een pop-up met HTML-inhoud, URL-koppeling naar een andere locatie binnen of buiten de app) verzenden. U kunt ook Android apparaten ring of Trillen met de Push. (Houd er rekening mee dat u de juiste moet machtigingen in de Android SDK-manifestbestand ring of een apparaat trillen.) Er is momenteel geen industrie standaard voor Android 'Big Picture' formaten, aangezien schermformaten op elk apparaat anders zijn, maar 400 x 100 foto's op bijna elke schermgrootte werken.

### <a name="delivery-types"></a>Levering-typen:
-    Alleen app: de kennisgeving wordt geleverd wanneer de gebruiker maakt geen gebruik van de toepassing.
- Buiten de app alleen melding moet een certificaat van Apple of Google (APNS of GCM certificaat).
- In-app alleen: de melding verschijnt alleen als de toepassing wordt uitgevoerd.
- De kennisgeving wordt het systeem voor verzending van Capptain te bereiken van de gebruiker. U kunt de visuele lay-out/weergave van uw push volledig aanpassen.
- Op elk gewenst moment: Deze optie zorgt ervoor dat u een melding die of de toepassing wordt uitgevoerd of niet verzenden.

 
![Reach-Campaign4][23]

### <a name="option-applies-to"></a>Optie is van toepassing op:
- Melding: Aankondigingen, Polls
 
## <a name="content"></a>Inhoud
U kunt de sectie inhoud wijzigen van de inhoud van uw mededelingen, Polls, gegevens duwt en tegels (alleen Windows Phone). De instelling van de inhoud van de Push-campagnes is specifiek voor de campagne. 

### <a name="see-also"></a>Zie ook
- [Documentatie van de UI - bereiken - Push-inhoud][Link 29]
 
![Reach-Campaign5][24]

## <a name="audience"></a>Doelgroep
De sectie doelgroep kunt u een standaard lijst met items te beperken uw campagne of limieten voor uw campagne op basis van aangepaste criteria definiëren. De standaard set opties te beperken uw publiek kunt u op nieuwe of oude gebruikers of alleen gebruikers native push push. U kunt ook een contingent beperken het aantal gebruikers aan wie u de push instellen. U kunt de expressie voor het filteren van uw campagne te nemen een of meer criteria voor Doelgebruikers handmatig bewerken. U kunt handmatig een doelgroep expressie typen. Deze expressie moet expliciet de relatie tussen de criteria definiëren. Een criterium wordt beschreven door een id die moet beginnen met een hoofdletter en mag geen spaties bevatten. De relatie tussen de criteria kan worden beschreven met behulp van 'en', 'of' niet' operatoren als '(',')'. Voorbeeld: "Criterion1 of (Criterion1 en niet Criterion2)".

> Opmerking: Met een grote doelgroep in campagnes worden opgenomen, de serverzijde Doelitems van het type scan is traag, vooral als u meerdere campagnes tegelijk starten.

- Alleen indien mogelijk, een campagne start tegelijk.
- Alleen op het meest vier campagnes starten tegelijk.
- Push-alleen voor de actieve gebruikers (checkbox "uitoefenen alleen gebruikers die kunnen worden bereikt met behulp van Native Push' en ' alleen actieve gebruikers ') zodat alleen gebruikers die nog steeds de app geïnstalleerd hebt en deze moet worden gescand.
Als uw doelgroep is gedefinieerd, kunt u de knop simulate om erachter te komen hoeveel gebruikers deze Push zal ontvangen. Hiermee berekent u het aantal bekende gebruikers mogelijk het doelwit van deze doelgroep (dit is een schatting op basis van een aselecte steekproef van gebruikers). Vergeet niet dat gebruikers die de toepassing hebt verwijderd ook deel van deze doelgroep uitmaken, maar kunnen niet worden bereikt.

### <a name="see-also"></a>Zie ook
- [Nieuwe Push criterium van UI - Reach - documentatie][Link 28]

![Reach-Campaign6][25]

### <a name="edit-expression"></a>Expressie bewerken
![Reach-Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Limiet voor die uw doelgroep optie is van toepassing op:
- Alleen een subset van gebruikers te verrichten: alle (aankondigingen, Polls, gegevens duwt, tegels)
- Alleen oude gebruikers uitoefenen: alle (aankondigingen, Polls, gegevens duwt, tegels)
- Alleen nieuwe gebruikers uitoefenen: alle (aankondigingen, Polls, gegevens duwt, tegels)
- Alleen niet-actieve gebruikers uitoefenen: aankondigingen, Polls, tegels
- Oefenen alleen actieve gebruikers: alle (aankondigingen, Polls, gegevens duwt, tegels)
- Alleen gebruikers die kunnen worden bereikt met behulp van Push-Native uitoefenen: aankondigingen, Polls
 
## <a name="time-frame"></a>Periode
Het tijdsbestek gedeelte kunt u instellen wanneer de push wordt verzonden of u kunt niets het tijdsbestek de campagne om onmiddellijk te beginnen. Houd er rekening mee dat met behulp van de eindgebruikers tijdzone kan de campagne start een dag eerder dan u voor de eindgebruikers in Azië verwacht en kleine batches van dit gereedschap duwt tegelijk totdat alle tijdzones in de wereld komen overeen met de periode die is ingesteld voor uw campagne verzonden. Met behulp van de eindgebruikers tijdzone kan ook leiden tot vertragingen in campagnes omdat er voor het aanvragen van de tijd van de telefoon voordat u begint met de push.

> Opmerking: Campagnes zonder een einddatum in de cache kan dit gereedschap duwt lokaal en nog steeds weergegeven nadat u handmatig complete campagnes. Om te voorkomen dat dit probleem specifiek een eindtijd voor campagnes.

### <a name="see-also"></a>Zie ook
- [Bereiken - hoe Tos – planning][Link 3] 
 
![Reach-Campaign8][27]

### <a name="settings-apply-to"></a>Instellingen van toepassing op:
- Periode: aankondigingen, Polls, tegels
 
## <a name="test"></a>Test
U kunt de sectie Test deze push verzenden naar uw eigen testapparaat voor het opslaan van de campagne. Als u een aangepaste taal voor deze campagne hebt geconfigureerd, kunt u de push testen in elke taal. U kunt instellen dat een testapparaat van "Mijn Account".
> Opmerking: Geen gegevens geregistreerd wanneer u met de knop 'test' server-side duwt, alleen gegevens vastgelegd voor echte push-campagnes.

### <a name="see-also"></a>Zie ook
- [Documentatie voor UI - Mijn Account][Link 14]
 
![Reach-Campaign9][28]

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
 

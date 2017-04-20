<properties
    pageTitle="Azure Active Directory identiteit bescherming woordenlijst | Microsoft Azure"
    description="Azure Active Directory identiteit beveiliging-Verklarende woordenlijst"
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingslek, beveiligingsbeleid, verklarende woordenlijst"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory identiteit beveiliging-Verklarende woordenlijst 


### <a name="at-risk-user"></a>Risico (gebruiker)  
Een gebruiker met een of meer actieve risico's. 

### <a name="atypical-sign-in-location"></a>Atypische locatie aanmelden   
Een teken in een geografische locatie die niet gebruikelijk is voor de specifieke gebruiker, zoals gebruikers of de huurder.

### <a name="azure-ad-identity-protection"></a>Azure identiteitsbeveiliging AD    
Een security module van Azure Active Directory biedt een geconsolideerde weergave van risico's en potentiële beveiligingsproblemen op het gebied van de identiteit van een organisatie.

### <a name="conditional-access"></a>Voorwaardelijke toegang  
Een beleid voor het beveiligen van toegang tot bronnen. Regels voor voorwaardelijke toegang in Azure Active Directory worden opgeslagen en worden geëvalueerd door Azure AD voordat deze toegang krijgen tot de bron.  Voorbeeld van regels opnemen op basis van de locatie, de toegang beperken verificatiemethode apparaat gezondheid of gebruiker.

### <a name="credentials"></a>Referenties     
Informatie over de identificatie en bewijs van identificatie die wordt gebruikt voor het verkrijgen van toegang tot lokale en netwerkbronnen. Voorbeelden van referenties zijn gebruikersnamen en wachtwoorden, smartcards en certificaten.

### <a name="event"></a>Gebeurtenis   
Een record van een activiteit in Azure Active Directory.

### <a name="false-positive-risk-event"></a>Fout-positieve (risicogebeurtenis) 
Een gebeurtenis Risicostatus handmatig instellen door een gebruiker identiteit bescherming, die aangeeft dat de risicogebeurtenis werd onderzocht en werd ten onrechte gemarkeerd als een onvoorziene gebeurtenis.

### <a name="identity"></a>Identiteit    
Een persoon of entiteit die moet worden gecontroleerd met behulp van verificatie op basis van criteria als een wachtwoord of certificaat.

### <a name="identity-risk-event"></a>Identiteit onvoorziene gebeurtenis     
AAD gebeurtenis die is gemarkeerd als afwijkende, identiteit bescherming en kan duiden op een identiteit is aangetast.

### <a name="ignored-risk-event"></a>Genegeerd (risicogebeurtenis)    
Een gebeurtenis Risicostatus handmatig instellen door een gebruiker identiteit bescherming, die aangeeft of de risicogebeurtenis is gesloten zonder een herstel actie te ondernemen.

### <a name="impossible-travel-from-atypical-locations"></a>Onmogelijk reizen van atypische locaties   
Een onvoorziene gebeurtenis geactiveerd wanneer twee aanmeldingen voor dezelfde gebruiker worden vastgesteld, waarbij ten minste één van een atypische aanmelden locatie en de tijd tussen de aanmeldingen korter is dan de tijd die minimaal nodig hebben zou om fysiek worden uitgewisseld tussen deze locaties.  

###<a name="investigation"></a>Onderzoek    
Tijdens het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een onvoorziene gebeurtenis om te beslissen of herstel of beperking stappen nodig zijn als begrijpen en hoe de identiteit is ingebroken en begrijpen hoe de gemanipuleerde identiteit werd gebruikt.

### <a name="leaked-credentials"></a>Uitgelekte referenties  

Een onvoorziene gebeurtenis geactiveerd wanneer de huidige gebruikersreferenties (gebruikersnaam en wachtwoord) worden gevonden door onze onderzoekers openbaar geboekt in het donker web.

### <a name="mitigation"></a>Risicobeperking  
Een actie te beperken of te elimineren van het vermogen van een aanvaller misbruik maken van een gemanipuleerde identiteit of het apparaat hersteld van de identiteit of het apparaat in een veilige staat. Een eerdere risico's die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

### <a name="multi-factor-authentication"></a>Meerledige verificatie 
Een verificatiemethode waarbij twee of meer verificatiemethoden, waaronder iets dat vereist dat de gebruiker heeft, dit certificaat; iets dat de gebruiker kent, zoals gebruikersnamen, wachtwoorden en wachtwoordzinnen; fysieke kenmerken, zoals een vingerafdruk; en persoonlijke kenmerken, zoals een persoonlijke handtekening.

### <a name="offline-detection"></a>Offline-detectie   
De opsporing van anomalieën en evaluatie van het risico van een gebeurtenis, zoals een aanmeldingspoging na het feit, voor een gebeurtenis die reeds is gebeurd.

### <a name="policy-condition"></a>Beleidsvoorwaarde    
Een deel van een waarin de entiteiten (groepen, gebruikers, apps, platforms apparaat, apparaat Staten, IP-bereiken, clienttypen) in het beleid worden opgenomen of uitgesloten van het beveiligingsbeleid.

### <a name="policy-rule"></a>Beleidsregel     
Het deel van een beveiligingsbeleid dat de omstandigheden die zou resulteren in het beleid en de maatregelen die worden getroffen beschrijft wanneer het beleid is geactiveerd.

### <a name="prevention"></a>Preventie  
Een actie om te voorkomen dat schade aan de organisatie door middel van misbruik van identiteit of het apparaat wordt vermoed of weet gevaar lopen. Een preventieve actie de identiteit of het apparaat niet wordt beveiligd en niet wordt opgelost door eerdere risico's.

### <a name="privileged-user"></a>Rechten (gebruiker)
Een gebruiker die op het moment van een onvoorziene gebeurtenis, permanente of tijdelijke admin machtigingen voor een of meer bron in Azure Active Directory, zoals een globale beheerder had factureringsbeheerder, servicebeheerder, Gebruikersbeheerder en beheerder wachtwoord. 

###<a name="real-time"></a>Real-time    
Raadpleeg Real-time detectie.

###<a name="real-time-detection"></a>Real-time detectie  
De opsporing van anomalieën en evaluatie van het risico van een gebeurtenis, zoals een aanmeldingspoging voordat de gebeurtenis is toegestaan om verder te gaan.

### <a name="remediated-risk-event"></a>(Risicogebeurtenis) verholpen     
Een gebeurtenis Risicostatus automatisch ingesteld door de bescherming van identiteiten, de onvoorziene gebeurtenis is verholpen met de standaard herstel-actie voor dit type risicogebeurtenis aangeeft. Bijvoorbeeld wanneer het wachtwoord van de gebruiker opnieuw wordt ingesteld, worden veel risico's die aangeven dat het vorige wachtwoord is ingebroken automatisch verholpen.

### <a name="remediation"></a>Herstel     
Een actie voor het beveiligen van een identiteit of een apparaat die eerder zijn verdacht of bekend worden aangetast. Een actie herstel de identiteit of het apparaat in een veilige toestand wordt hersteld en vorige risico's die zijn gekoppeld aan de identiteit of het apparaat wordt opgelost.

### <a name="resolved-risk-event"></a>(Risicogebeurtenis) opgelost   
Een gebeurtenis Risicostatus handmatig instellen door een gebruiker identiteit bescherming, die aangeeft dat de gebruiker heeft een passende oplossing actie buiten de bescherming van de identiteit en de risicogebeurtenis wordt geacht gesloten.

###<a name="risk-event-status"></a>Status van risico    
Een eigenschap van een onvoorziene gebeurtenis die aangeeft of de gebeurtenis actief is en als gesloten, de reden voor het afsluiten.

###<a name="risk-event-type"></a>Type risico-gebeurtenis  
Een categorie voor de onvoorziene gebeurtenis, die het type aangeeft van die riskant worden beschouwd de gebeurtenis heeft veroorzaakt.

###<a name="risk-level-risk-event"></a>Risiconiveau (risico event)  
Een indicatie van de ernst van de risicogebeurtenis voor identiteitsbeveiliging acties prioriteit (hoog, Gemiddeld of laag) die zij nemen om te beperken het risico dat hun organisatie. 

###<a name="risk-level-sign-in"></a>Risiconiveau (aanmelden) 
Een indicatie (hoog, Gemiddeld of laag) van de kans dat voor een specifieke aanmelden, iemand anders probeert te gebruiken van de identiteit van de gebruiker.

###<a name="risk-level-user-compromise"></a>Risiconiveau (gebruiker inbreuk) 
Een indicatie (hoog, Gemiddeld of laag) van de kans dat een identiteit is ingebroken.

### <a name="risk-level-vulnerability"></a>Risiconiveau (beveiligingslek)  
Een indicatie van de ernst van het probleem voor identiteitsbeveiliging acties prioriteit (hoog, Gemiddeld of laag) die zij nemen om te beperken het risico dat hun organisatie.

### <a name="secure-identity"></a>Beveiliging (identiteit)   
Herstel maatregelen zoals een wachtwoord wijzigen of de computer als een potentieel aangetaste identiteit weer in een compromisloze staat reimaging.

### <a name="security-policy"></a>Beveiligingsbeleid
Een collectie van beleidsregels en de voorwaarde. Een beleid kan worden toegepast op entiteiten, zoals gebruikers, groepen, apps, apparaten, platforms apparaat, apparaat Staten, IP-bereiken en Auth2.0 clienttypen. Wanneer een beleid is ingeschakeld, wordt dit geëvalueerd als een token voor een resource is uitgegeven door een entiteit die is opgenomen in het beleid.

### <a name="sign-in-v"></a>Log in (v) 
Voor het verifiëren van een identiteit in Azure Active Directory.

### <a name="sign-in-n"></a>Aanmelden (n) 
Het proces of de actie van de verificatie van een identiteit in Azure Active Directory en de gebeurtenis die deze bewerking worden vastgelegd.

###<a name="sign-in-from-anonymous-ip-address"></a>Aanmelden van anonieme IP-adres    
Een risicogebeurtenis wordt geactiveerd na een geslaagde aanmelden vanaf IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres.

###<a name="sign-in-from-infected-device"></a>Aanmelden van een geïnfecteerd apparaat 
Een onvoorziene gebeurtenis geactiveerd wanneer een aanmelden afkomstig van een IP-adres waarvan bekend is dat door een of meer beschadigde apparaten die actief probeert is te communiceren met een bot-server worden gebruikt.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Aanmelden vanaf IP-adres met verdachte activiteiten 
Een onvoorziene gebeurtenis geactiveerd nadat een geslaagde aanmelden van een IP adres met een groot aantal mislukte inlogpogingen tussen meerdere gebruikersaccounts gedurende een korte periode.

###<a name="sign-in-from-unfamiliar-location"></a>Aanmelden van onbekende locatie 
Een onvoorziene gebeurtenis geactiveerd wanneer een gebruiker met succes zich vanaf een nieuwe locatie (IP-, Latitude/lengtegraad en ASN aanmeldt).

###<a name="sign-in-risk"></a>-In gevaar     
Zie risico niveau (aanmelden)

###<a name="sign-in-risk-policy"></a>Risico-in beleid  
Een beleid van voorwaardelijke toegang die de risico's voor een specifieke aanmelden evalueert en op basis van vooraf gedefinieerde voorwaarden en beperkingen van toepassing is.

###<a name="user-compromise-risk"></a>Inbreuk op het risico van gebruiker     
Zie risico niveau (gebruiker inbreuk)

###<a name="user-risk"></a>Risico van de gebruiker    
Zie risico niveau (gebruiker inbreuk).

###<a name="user-risk-policy"></a>Gebruikersbeleid voor risico     
Een beleid van voorwaardelijke toegang die van mening is het aanmelden en op basis van vooraf gedefinieerde voorwaarden en beperkingen van toepassing is.

###<a name="users-flagged-for-risk"></a>Gebruikers die zijn gemarkeerd voor risico   
Gebruikers die beschikken over de risico's die de status actief of verholpen

###<a name="vulnerability"></a>Beveiligingslek met betrekking tot    
Een configuratie of de voorwaarde in Azure Active Directory die vatbaar zijn voor misbruik maakt van de map of bedreigingen.


## <a name="see-also"></a>Zie ook

- [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md)

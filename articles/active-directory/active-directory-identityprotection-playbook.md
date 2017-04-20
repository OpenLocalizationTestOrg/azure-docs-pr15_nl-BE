<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging playbook | Microsoft Azure"
    description="Ga na hoe identiteitsbeveiliging Azure AD u beperken het vermogen van een aanvaller misbruik maken van een gemanipuleerde identiteit of een apparaat voor het beveiligen van een identiteit of een apparaat dat eerder verdacht of bekend worden aangetast."
    services="active-directory"
    keywords="Azure active directory identiteitsbeveiliging, cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingslek, beveiligingsbeleid beheren"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identiteitsbeveiliging playbook 

Deze playbook kunt u:

- Gegevens in de omgeving identiteitsbeveiliging vullen met ter simulering van risico's en zwakke plekken
- Instellen van het beleid van voorwaardelijke toegang op basis van het risico en de gevolgen van dit beleid


## <a name="simulating-risk-events"></a>Risico's te simuleren

Deze sectie bevat stappen voor het simuleren van de volgende typen voor risico-gebeurtenis:

- Aanmeldingen van anonieme IP-adressen (eenvoudig)
- Aanmeldingen van onbekende locaties (gemiddeld)
- Onmogelijk reizen naar atypische locaties (moeilijk)

Andere risico's kunnen niet op een veilige manier worden gesimuleerd.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>Aanmeldingen van anonieme IP-adressen

Dit type risico gebeurtenis identificeert gebruikers die zich heeft aangemeld vanaf een IP-adres dat is geïdentificeerd als een anonieme proxy IP-adres. Deze proxy's worden gebruikt door mensen die u wilt verbergen IP-adres van het apparaat en kwade bedoelingen kan worden gebruikt.

**De volgende stappen uitvoeren om te simuleren een aanmelden van een anonieme IP**:

1.  De [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en)downloaden.
2.  Met de Tor Browser, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Voer de referenties van de account die u wilt weergeven in de lijst van **aanmeldingen van anonieme IP-adressen** .

Het aanmelden wordt weergegeven op het dashboard voor bescherming van identiteiten binnen 5 minuten. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Aanmeldingen van onbekende locaties

Het risico op onbekende locaties is een real-time-in evaluatiemechanisme acht locaties-in het verleden (IP-, Latitude / lengtegraad en ASN) om te bepalen locaties voor nieuwe / niet bekend. Het systeem slaat vorige IPs, Latitude / lengtegraad en ASN's van een gebruiker en wordt deze vertrouwde locaties worden beschouwd. Een locatie-in wordt beschouwd als niet bekend als de locatie-in niet overeenkomt met de bestaande vertrouwde locaties.

Bescherming van de identiteit Azure Active Directory:  

 - heeft een eerste leren periode van 14 dagen gedurende welke het heeft vlag niet nieuwe locaties als onbekende locaties.
 - aanmeldingen van bekende apparaten en locaties die geografisch dicht bij een bestaande locatie bekend negeert.

Om na te bootsen onbekende locaties, hebt u zich aanmelden via een locatie en een apparaat dat de rekening is niet ondertekend in uit voordat. 


**De volgende stappen uitvoeren om te simuleren een aanmelden vanaf een onbekende locatie**:

1.  Kies een account met ten minste 14 dagen-in de geschiedenis. 

2.  Voer een:
    
    een. Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com) en voer de referenties van de account die u wilt simuleren de risicogebeurtenis voor tijdens het gebruik van een VPN.

    b. Vraag een geassocieerde deelneming in een andere locatie aan te melden met de referenties van de account (niet aanbevolen).

Het aanmelden wordt weergegeven op het dashboard voor bescherming van identiteiten binnen 5 minuten.
 
### <a name="impossible-travel-to-atypical-location"></a>Onmogelijk reizen naar atypische locatie
De voorwaarde onmogelijk reizen simuleren is moeilijk omdat het algoritme leren wieden van false-positieven zoals onmogelijk reizen van bekende apparaten of aanmeldingen van VPN-verbindingen die worden gebruikt door andere gebruikers in de map computer gebruikt. Bovendien vereist het algoritme-in geschiedenis van 3 tot 14 dagen voor de gebruiker voordat deze begint met het genereren van risico's.

**Om te simuleren een onmogelijk reizen naar atypische locatie, de volgende stappen uitvoeren**:

1.  Met uw standaard browser, Ga naar [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Voer de referenties van de account die u wilt genereren een gebeurtenis onmogelijk reizen risico voor.

3.  De gebruikersagent wijzigen. U kunt de gebruikersagent van Internet Explorer wijzigen van ontwikkelprogramma's of wijzigen van de gebruikersagent in Firefox of Chrome met behulp van een invoegtoepassing voor de gebruikersagent schakelbaar.

4.  Uw IP-adres wijzigen. U kunt uw IP-adres wijzigen met behulp van een VPN, een invoegtoepassing Tor, of draait om een nieuwe machine in Azure in een ander datacenter.

5.  Inloggen en binnen een paar minuten na de vorige inloggen [https://myapps.microsoft.com](https://myapps.microsoft.com) met dezelfde referenties als voorheen.

Het aanmelden wordt weergegeven in het dashboard voor bescherming van identiteiten binnen 2-4 uur.<br>
Vanwege de complexe machine learning betrokken modellen, is er een kans dat het zal niet krijgen opgepikt.<br> U kunt deze stappen voor meerdere accounts met Azure AD repliceren.


## <a name="simulating-vulnerabilities"></a>Beveiligingslekken simuleren 
Beveiligingslekken zijn zwakke punten in een omgeving met Azure AD dat kan worden misbruikt door een slechte acteur. Op dit moment zijn 3 soorten beveiligingslekken opgehaald in Azure AD identiteitsbeveiliging die gebruikmaken van andere functies van AD Azure. Deze beveiligingslekken op het dashboard identiteitsbeveiliging automatisch weergegeven als deze functies zijn ingesteld.

-   Azure AD [meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)
-   AD Azure [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD [beschermde Identity Management](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Inbreuk op het risico van gebruiker

**Testen van inbreuk op het risico voor de gebruiker, de volgende stappen uitvoeren**:

1.  Aanmelden bij [https://portal.azure.com](https://portal.azure.com) met globale administrator-referenties voor de huurder.

2.  Ga naar de **bescherming van de identiteit**. 

3.  Klik op **Instellingen**op de belangrijkste **identiteitsbeveiliging Azure AD** -blade. 

4.  Klik op de **Instellingen voor Portal** -blade onder **regels voor** **gebruiker risico in gevaar brengen**. 

5.  Op het blad **aanmelden risico** **regel inschakelen** uitschakelen en klik vervolgens op instellingen **Opslaan** .

6.  Voor een bepaalde gebruikersaccount simuleren een onbekende locaties of anonieme IP onvoorziene gebeurtenis. Dit zal niveau van de risico's voor die gebruiker verhogen op **Normaal**.

7.  Wacht een paar minuten en controleer vervolgens of het toegangsniveau gebruiker voor de gebruiker **Normaal**.

8.  Ga naar de **Instellingen voor Portal** -blade.

9.  Selecteer op de blade **Gebruiker inbreuk risico** onder **regel inschakelen**, **op** . 

10. Selecteer een van de volgende opties:

    een. Wilt blokkeren, selecteert u **Normaal** onder **blok inloggen**.

    b. Als u beveiligd wachtwoord wijzigen, selecteert u **Normaal** onder **meerledige verificatie vereist**.

13. Klik op **Opslaan**.

14. Meldt een gebruiker met een verhoogde risiconiveau kunt u nu voorwaardelijke toegang op basis van risico's testen. Als het risico van de gebruiker normaal is, is afhankelijk van de configuratie van het beleid, het aanmelden worden geblokkeerd of u gedwongen om uw wachtwoord te wijzigen. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>-In gevaar

 
**Test een teken in gevaar door de volgende stappen uitvoeren:**

1.  Aanmelden bij [https://portal.azure.com](https://portal.azure.com) met globale administrator-referenties voor de huurder.

2.  Ga naar de **bescherming van de identiteit**.

3.  Klik op **Instellingen**op de belangrijkste **identiteitsbeveiliging Azure AD** -blade. 

4.  Klik op de **Instellingen voor Portal** -blade onder **regels** **risico aanmelden**.

5.  Selecteer op de bladeserver **aanmelden risico ** **op** onder **regel inschakelen**. 

7.  Selecteer een van de volgende opties:

    een. Wilt blokkeren, selecteert u **Normaal** onder **blok inloggen**

    b. Als u beveiligd wachtwoord wijzigen, selecteert u **Normaal** onder **meerledige verificatie vereist**.

8.  Als u wilt blokkeren, selecteer Normaal onder blok inloggen.

9.  Om het meerledige verificatie afdwingen, selecteert u **Normaal** onder **meerledige verificatie vereist**.

10. Klik op **Opslaan**.

11. Nu kunt u voorwaardelijke toegang op basis van risico's door te simuleren de onbekende locaties of anonieme IP-risico's, omdat ze beide **Gemiddeld** risico's testen.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Zie ook

 - [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md)

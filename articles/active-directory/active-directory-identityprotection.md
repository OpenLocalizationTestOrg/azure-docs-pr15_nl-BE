<properties
    pageTitle="Azure Active Directory identiteitsbeveiliging | Microsoft Azure"
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
    ms.date="10/26/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection"></a>Identiteitsbeveiliging Azure Active Directory 

Azure Active Directory identiteitsbeveiliging is een functie van de P2 Azure AD Premium editie biedt een geconsolideerde weergave van risico's en potentiële beveiligingsproblemen op het gebied van de identiteit van uw organisatie. Microsoft heeft zijn beveiligen van cloud-gebaseerde identiteiten voor meer dan een decennium en met AD identiteitsbeveiliging Azure, Microsoft is deze dezelfde bescherming systemen beschikbaar maken voor zakelijke klanten. Bescherming van identiteiten maakt gebruik van bestaande Azure AD afwijking functies (beschikbaar via Azure AD afwijkende activiteitsrapporten) en introduceert nieuwe risico gebeurtenistypen die in real-time kunnen anomalieën.



##<a name="getting-started"></a>Aan de slag

De overgrote meerderheid van inbreuken op de beveiliging plaatsvinden als onbevoegden toegang krijgen tot een omgeving door te stelen van de identiteit van een gebruiker. Hackers zijn steeds meer effectief bij gebruik van inbreuken op de derde partij en het gebruik van geavanceerde phishingaanvallen geworden. Als een aanvaller toegang tot zelfs een lage bevoorrechte gebruikersaccount krijgt, is het betrekkelijk eenvoudig om toegang te krijgen tot bedrijfsbronnen via een zijwaartse beweging belangrijk. Het is daarom essentieel voor alle identiteiten beschermen en als een identiteit is ingebroken, proactief te voorkomen dat de gemanipuleerde identiteit geschonden. 

Ontdekking van een gemanipuleerde identiteiten is geen eenvoudige taak. Gelukkig identiteitsbeveiliging kan helpen: identiteitsbeveiliging maakt gebruik van geavanceerde machine learning algoritmen en methodiek anomalieën en risico-gebeurtenissen die erop wijzen kunnen dat een identiteit is ingebroken.
 
Met behulp van deze gegevens, bescherming van identiteiten genereert rapporten en waarschuwingen, waarmee u deze risico's onderzoeken en passend herstel of beperking actie ondernemen.
 
Maar Azure Active Directory identiteitsbeveiliging is meer dan een hulpmiddel voor bewaking en rapportage. Op basis van risico's, berekend identiteit bescherming het risiconiveau van een gebruiker voor elke gebruiker, zodat u kunt configureren op basis van risico beleid ter bescherming van de identiteit van uw organisatie automatisch.  Deze risico-beleid, naast andere besturingselementen voorwaardelijke toegang is geleverd door Azure Active Directory en EMS, kunnen automatisch blokkeren of bieden adaptief herstel acties opnieuw instellen van wachtwoorden en meerledige verificatie afdwingen.  

####<a name="explore-identity-protections-capabilities"></a>Ontdek de mogelijkheden van de bescherming van de identiteit 

**Opsporen van risico's en riskant accounts:**  

- 6 risico gebeurtenistypen met machine learning en heuristische regels vaststellen 

- Berekening van de gebruiker risico niveaus

- Aangepaste aanbevelingen ter verbetering van de algehele beveiliging houding beveiligingslekken markeert bieden



**Onderzoek naar risico's:**

- Verzenden van meldingen voor risico 's

- Onderzoek naar risico's met behulp van relevante en contextuele informatie

- Biedt eenvoudige workflows bijhouden van onderzoek

- Eenvoudige toegang tot herstel acties zoals het wachtwoord opnieuw instellen



**Beleid voor voorwaardelijke toegang op basis van risico's:**

- Beleid riskant aanmeldingen beperken door het blokkeren van aanmeldingen of uitdagingen meerledige verificatie vereisen.

- Beleid blokkeren of beveiligen riskant gebruikersaccounts

- Beleid voor gebruikers om te registreren voor een meerledige verificatie vereist


## <a name="detection-and-risk"></a>Detectie- en risico

### <a name="risk-events"></a>Risico 's

Risico's zijn gebeurtenissen die zijn gemarkeerd als verdacht identiteit bescherming en geven aan dat een identiteit is ingebroken. Zie voor een volledige lijst met risico's, [soorten risico's door Azure Active Directory identiteitsbeveiliging gedetecteerd](active-directory-identityprotection-risk-events-types.md). 


### <a name="risk-level"></a>Risiconiveau

Het risiconiveau voor een onvoorziene gebeurtenis is een indicatie van de ernst van de risicogebeurtenis (hoog, Gemiddeld of laag). Het risiconiveau helpt gebruikers bij identiteitsbeveiliging prioriteit van de acties die moet daarbij het risico verkleinen tot hun organisatie. De ernst van de risicogebeurtenis vertegenwoordigt de sterkte van het signaal identiteit compromissen, gecombineerd met de hoeveelheid ruis die het meestal geeft te voorspellen. 

- **Hoog**: hoge betrouwbaarheid en zeer ernstig risico-gebeurtenis. Deze gebeurtenissen zijn de sterke indicatoren die de identiteit van de gebruiker meer veilig is en negatieve gevolgen hebben voor gebruikersaccounts moeten direct worden verholpen.

- **Gemiddeld**: zeer ernstig zijn, maar minder vertrouwen onvoorziene gebeurtenis, of vice versa. Deze gebeurtenissen zijn mogelijk schadelijke en negatieve gevolgen hebben voor gebruikersaccounts moeten worden verholpen.

- **Laag**: lage betrouwbaarheid en lage prioriteit onvoorziene gebeurtenis. Deze gebeurtenis kan een onmiddellijke actie vereist, maar in combinatie met andere risico's kan bieden een sterke aanwijzing dat de identiteit wordt aangetast. 


![Risiconiveau] (./media/active-directory-identityprotection/01.png "Risiconiveau")

 

Risico's worden ofwel geïdentificeerd in **real-time**, of achteraf worden verwerkt nadat de risicogebeurtenis al is (offline) plaats. Momenteel de meeste risico's in identiteitsbeveiliging off line worden berekend en weergegeven in de bescherming van identiteiten binnen 2-4 uur. Terwijl geëvalueerd in real-time, de real-time risico's wordt weergegeven in de Console identiteit bescherming binnen 5-10 minuten.

Enkele oudere clients ondersteunen momenteel geen real-time risico gebeurtenis detecteren en tegenhouden. Als gevolg hiervan aanmeldingen van deze clients niet gedetecteerd of kunnen in real-time.


## <a name="investigation"></a>Onderzoek
Uw reis door identiteitsbeveiliging begint meestal met het dashboard voor bescherming van identiteiten. 

![Herstel] (./media/active-directory-identityprotection/1000.png "Herstel")

Het dashboard hebt die u toegang tot:
 
- Rapporten, zoals **gebruikers die zijn gemarkeerd voor risico's**, **risico's** en **zwakke plekken**
- Instellingen, zoals de configuratie van het **Beveiligingsbeleid**, **berichten** en **registratie meerledige verificatie**
 

Meestal is het startpunt voor onderzoek, het proces is voor het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een onvoorziene gebeurtenis om te beslissen of herstel of beperking stappen nodig zijn, en hoe de identiteit is ingebroken en begrijpen hoe de gemanipuleerde identiteit werd gebruikt.

U kunt uw onderzoeksactiviteiten aan de die bescherming van Azure Active Directory worden verzonden per e-mail [meldingen](active-directory-identityprotection-notifications.md) verbinden.

De volgende secties vindt u meer informatie en de stappen die zijn gerelateerd aan een onderzoek.  



## <a name="what-is-a-user-risk-level"></a>Wat is een risico op gebruikersniveau?

Een risico op gebruikersniveau is een indicatie van de kans dat de identiteit van de gebruiker meer veilig is (hoog, Gemiddeld of laag). Deze wordt berekend op basis van de risico's van gebruikers die gekoppeld aan de identiteit van de gebruiker zijn. 

De status van een onvoorziene gebeurtenis is **actief** of **afgesloten**. Alleen risico's die **actief** bijdragen aan de gebruiker risico berekening. 

Niveau van de risico's wordt berekend met behulp van de volgende ingangen:

- Actieve risico's invloed op de gebruiker
- Niveau van het risico van deze gebeurtenissen 
- Of herstel acties zijn genomen 

![Risico's van gebruiker] (./media/active-directory-identityprotection/1001.png "Risico's van gebruiker")



U kunt de gebruiker risico niveaus gebruiken voor het maken van beleid voor voorwaardelijke toegang om te verhinderen dat gebruikers riskant aanmelden of zorgen dat ze veilig hun wachtwoord te wijzigen. 


## <a name="closing-risk-events-manually"></a>Risico's handmatig sluiten

In de meeste gevallen duurt herstel acties zoals een veilig wachtwoord opnieuw instellen op automatisch sluiten risico's. Echter, dit is niet altijd mogelijk.  
Dit is bijvoorbeeld het geval wanneer:

- Actieve risico's van een gebruiker is verwijderd.
- Een onderzoek blijkt dat een risicogebeurtenis gemeld heeft zijn uitgevoerd door de rechtmatige gebruiker

Omdat de risico's die **actief** bijdragen aan de gebruiker risico berekening, moet u wellicht handmatig een risiconiveau verlagen door risico's handmatig sluiten.  
In de loop van het onderzoek kunt u een van deze acties te wijzigen van de status van een onvoorziene gebeurtenis nemen:

![Acties] (./media/active-directory-identityprotection/34.png "Acties")

- **Oplossen** - als u denkt dat de risicogebeurtenis moet worden beschouwd en na onderzoek van een onvoorziene gebeurtenis, u een passende oplossing actie buiten identiteitsbeveiliging heeft markeren gesloten, de gebeurtenis als omgezet. Gebeurtenissen van de risicogebeurtenis status wordt ingesteld in afgesloten en de risicogebeurtenis niet langer draagt bij tot het risico van de gebruiker omgezet.

- **Markeren als fout-positieve** - In sommige gevallen mag een onvoorziene gebeurtenis onderzoeken en ontdekken dat onjuist is gemarkeerd als een riskant. Verkleint u het aantal dergelijke gevallen door het op de risicogebeurtenis fout-positieve markering. Dit helpt de computer leren algoritmen ter verbetering van de classificatie van soortgelijke gebeurtenissen in de toekomst. De status van fout-positieve gebeurtenissen wordt **gesloten** en ze niet langer draagt bij tot het risico van de gebruiker.

- **Negeren** - als u niet elke actie herstelmaatregelen genomen, maar de gebeurtenis risico wilt worden verwijderd uit de actieve lijst, kunt u een onvoorziene gebeurtenis negeren markeren en de gebeurtenisstatus wordt gesloten. Gebeurtenissen genegeerd bijdragen niet tot het risico van de gebruiker. Deze optie moet alleen worden gebruikt onder bijzondere omstandigheden. 

- **Opnieuw activeren** - risico's die handmatig zijn afgesloten (door het kiezen van **oplossen**, **valse melding**of **negeren**) kan worden gereactiveerd, instellen van de gebeurtenisstatus te **activeren**. Opnieuw geactiveerde risico's bijdragen aan de gebruiker risico niveau berekening. Risico's die zijn gesloten via doorvoeren (zoals een veilig wachtwoord opnieuw instellen) kunnen niet opnieuw worden geactiveerd. 




**Het bijbehorende dialoogvenster openen**:

1. Klik op het blad **identiteitsbeveiliging Azure AD** onder **onderzoeken**, **risico's**.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1002.png "Wachtwoord handmatig opnieuw instellen")

2. Klik in de lijst met **risico's** op een risico.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1003.png "Wachtwoord handmatig opnieuw instellen")

2. Klik met de rechtermuisknop op een gebruiker op de bladeserver risico.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1004.png "Wachtwoord handmatig opnieuw instellen")



### <a name="closing-all-risk-events-for-a-user-manually"></a>Alle risico's voor een gebruiker sluiten handmatig

In plaats van handmatig sluiten individueel risico's voor een gebruiker, biedt Azure Active Directory identiteit bescherming u ook een methode voor het sluiten van alle gebeurtenissen voor een gebruiker met één klik.


![Acties] (./media/active-directory-identityprotection/2222.png "Acties")

Als u op **negeren van alle gebeurtenissen**, alle gebeurtenissen zijn gesloten en de gebruiker niet meer risico.



## <a name="remediating-user-risk-events"></a>Beveiligingsstatus gebruiker risico 's

Een herstel is een actie voor het beveiligen van een identiteit of een apparaat dat eerder verdacht of bekend worden aangetast. Een actie herstel de identiteit of het apparaat in een veilige toestand wordt hersteld en vorige risico's die zijn gekoppeld aan de identiteit of het apparaat wordt opgelost.

Als een gebruiker risico's te verhelpen, kunt u het volgende doen:

- Uitvoeren van een veilig wachtwoord opnieuw instellen op een gebruiker risico's handmatig te verhelpen 

- Een gebruikersbeveiligingsbeleid van risico's te verminderen of verhelpen risico's van gebruikers automatisch configureren

- Afbeelding opnieuw geïnfecteerd apparaat  


### <a name="manual-secure-password-reset"></a>Veilig wachtwoord handmatig opnieuw instellen

Een veilig wachtwoord reset is een effectieve oplossing voor veel risico's en wanneer uitgevoerd, automatisch sluit u deze risico's en het risico op gebruikersniveau worden opnieuw berekend. Het dashboard voor bescherming van identiteiten kunt u een wachtwoord voor een riskante gebruiker opnieuw starten. 

Het bijbehorende dialoogvenster biedt twee verschillende methoden om uw wachtwoord opnieuw instellen:

**Wachtwoord opnieuw instellen** - Selecteer **gebruiker moet het wachtwoord opnieuw instellen** zodat de gebruiker zelf herstellen als de gebruiker is geregistreerd voor een meerledige verificatie. Tijdens de gebruiker de volgende aanmelden, wordt de gebruiker zijn vereist voor het oplossen van een meerledige verificatie uitdaging is en vervolgens gedwongen het wachtwoord te wijzigen. Deze optie is niet beschikbaar als de gebruikersaccount nog niet geregistreerde meerledige verificatie.

Het **tijdelijke wachtwoord** - Selecteer **een tijdelijk wachtwoord genereren** onmiddellijk het bestaande wachtwoord ongeldig en maakt een nieuw tijdelijk wachtwoord voor de gebruiker. Het nieuwe tijdelijke wachtwoord verzenden naar een alternatief e-mailadres voor de gebruiker of naar de manager van de gebruiker. Het wachtwoord is tijdelijk en wordt de gebruiker gevraagd het wachtwoord bij het aanmelden.


![Beleid] (./media/active-directory-identityprotection/1005.png "Beleid")


**Het bijbehorende dialoogvenster openen**:

1. Klik op het blad **Azure AD identiteitsbeveiliging** **gebruikers gemarkeerd voor risico**.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1006.png "Wachtwoord handmatig opnieuw instellen")


2. Selecteer ten minste één risico's van een gebruiker uit de lijst met gebruikers.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1007.png "Wachtwoord handmatig opnieuw instellen")


2. Klik op **wachtwoord opnieuw instellen**op de bladeserver gebruiker.

    ![Wachtwoord handmatig opnieuw instellen] (./media/active-directory-identityprotection/1008.png "Wachtwoord handmatig opnieuw instellen")





## <a name="user-risk-security-policy"></a>Gebruikersbeveiligingsbeleid risico

Een gebruikersbeveiligingsbeleid van risico's is een beleid van voorwaardelijke toegang die wordt geëvalueerd als het niveau van de risico's op een specifieke gebruiker en herstelmaatregelen en risicobeperking acties op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.


![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1009.png "Gebruikersbeleid voor ridk")


Azure AD identiteitsbeveiliging kunt u de bestrijdings- en herstel van gebruikers die zijn gemarkeerd voor risico doordat u kunt beheren:

- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1010.png "Gebruikersbeleid voor ridk")


- Stelt de gebruiker gevaar drempel op (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd: 

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1011.png "Gebruikersbeleid voor ridk")


- De besturingselementen worden afgedwongen wanneer het beleid wordt ingesteld:

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1012.png "Gebruikersbeleid voor ridk")


- De status van uw beleid gaan:

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/403.png "Registratie van de MVR gesloten")


- Bekijken en beoordelen van de gevolgen van een wijziging voor activeren:

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1013.png "Gebruikersbeleid voor ridk")


Kiezen van een **hoge** drempel vermindert het aantal keren dat een beleid wordt geactiveerd en de overlast voor gebruikers tot een minimum beperkt.
Ongerekend echter **laag** en **Normaal** gebruikers gemarkeerd voor risico van het beleid, die mogelijk niet veilig identiteiten of apparaten die eerder zijn verdacht of bekend worden aangetast.

Bij het instellen van het beleid,

- Voorkomen dat gebruikers die kunnen genereren veel false-positieven (ontwikkelaars, analisten beveiliging)

- Gebruikers in landen waar het beleid inschakelen is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)

- Gebruik een **hoge** drempel tijdens de eerste beleid zetten, of als u moet uitdagingen zichtbaar voor eindgebruikers minimaliseren.

- Een **lage** drempel gebruiken als uw organisatie meer beveiliging vereist. Selecteren van een **lage** drempel introduceert extra gebruiker aanmelden uitdagingen, maar verbeterde beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is een regel voor een **Gemiddeld** drempel een evenwicht tot stand tussen gebruiksgemak en beveiliging configureren.

Zie voor een overzicht van de bijbehorende gebruikerservaring:

- [Compromised account recovery stroom](active-directory-identityprotection-flows.md#compromised-account-recovery).  

- [Compromised account geblokkeerd stroom](active-directory-identityprotection-flows.md#compromised-account-blocked).  


**Het bijbehorende dialoogvenster openen**:

1. Klik op het blad **identiteitsbeveiliging Azure AD** in de sectie **configureren** **gebruiker risico beleid**.

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1009.png "Gebruikersbeleid voor ridk")






## <a name="mitigating-user-risk-events"></a>Beperkende gebruiker risico 's
Beheerders kunnen een risico van gebruikersbeveiligingsbeleid voorkomen dat gebruikers bij het aanmelden afhankelijk van het risiconiveau instellen. 

Blokkeren van een teken in:
 
- Hiermee voorkomt u dat het genereren van nieuwe gebruiker risico's voor de desbetreffende gebruiker

- Hiermee kunnen beheerders handmatig doorvoeren van de risico's op het gebied van de identiteit van de gebruiker en een veilige status herstellen



## <a name="what-is-a-sign-in-risk-level"></a>Wat is een niveau-in gevaar?

Een niveau-in gevaar is een indicatie van de kans dat voor een specifieke aanmelden, iemand anders probeert te verifiëren van de identiteit van de gebruiker (hoog, Gemiddeld of laag). Het risiconiveau-in wordt geëvalueerd op het moment van een aanmelden en acht de risico's en indicatoren gevonden in real-time voor die specifieke aanmelden. 

## <a name="mitigating-sign-in-risk-events"></a>Beperkende-in risico 's 
Een beperking is een actie voor het beperken van het vermogen van een aanvaller misbruik maken van een gemanipuleerde identiteit of het apparaat hersteld van de identiteit of het apparaat in een veilige staat. Een eerdere risico-in gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

U kunt voorwaardelijke toegang in Azure AD identiteitsbeveiliging automatisch aanmelden risico's te beperken. Met dit beleid, u rekening houden met het risiconiveau van de gebruiker of het aanmelden riskant aanmeldingen blokkeren of de gebruiker meerledige verificatie uitvoeren. Deze acties kunnen voorkomen dat een aanvaller misbruik van een gestolen identiteit te beschadigen en enige tijd voor het beveiligen van de identiteit kunnen geven. 


## <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid-in gevaar

Een risico-in is een beleid van voorwaardelijke toegang die het risico op een specifieke aanmelden evalueert en op basis van vooraf gedefinieerde voorwaarden en beperkingen van toepassing is.

![Beleid - in gevaar] (./media/active-directory-identityprotection/1014.png "Beleid - in gevaar")


Azure AD identiteitsbeveiliging helpt u bij het beperken van risicovolle aanmeldingen die:

- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![Beleid - in gevaar] (./media/active-directory-identityprotection/1015.png "Beleid - in gevaar")


- De risico-in niveau drempel instellen (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd: 

    ![Beleid - in gevaar] (./media/active-directory-identityprotection/1016.png "Beleid - in gevaar")


- De besturingselementen worden afgedwongen wanneer het beleid wordt ingesteld:  

    ![Beleid - in gevaar] (./media/active-directory-identityprotection/1017.png "Beleid - in gevaar")
    

- De status van uw beleid gaan:

    ![Registratie van de MVR gesloten] (./media/active-directory-identityprotection/403.png "Registratie van de MVR gesloten")

- Bekijken en beoordelen van de gevolgen van een wijziging voor activeren: 

    ![Beleid - in gevaar] (./media/active-directory-identityprotection/1018.png "Beleid - in gevaar")


### <a name="what-you-need-to-know"></a>Wat u moet weten

U kunt een beveiligingsbeleid Aanmelden risico meerledige verificatie configureren:

![Beleid - in gevaar] (./media/active-directory-identityprotection/1017.png "Beleid - in gevaar")

Echter, om veiligheidsredenen deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor een meerledige verificatie. Als de voorwaarde meerledige verificatie vereist voor een gebruiker die nog niet is geregistreerd voor een meerledige verificatie is voldaan, wordt de gebruiker is geblokkeerd. 

Beste, als u wilt meerledige verificatie vereisen voor risicovolle aanmeldingen, moet u:

1. Het [registratiebeleid meerledige verificatie](#multi-factor-authentication-registration-policy) van de betrokken gebruikers inschakelen.
2. De betreffende gebruikers om aan te melden in een sessie niet riskant voor het uitvoeren van een registratie van de MVR gesloten vereisen

U deze stappen uitvoert, zorgt u ervoor dat meerledige verificatie vereist is voor een riskante aanmelden. 


### <a name="best-practices"></a>Aanbevolen procedures

 
Kiezen van een **hoge** drempel vermindert het aantal keren dat een beleid wordt geactiveerd en de overlast voor gebruikers tot een minimum beperkt.  
 
Het omvat echter niet **laag** en **Normaal** aanmeldingen gemarkeerd voor risico van het beleid, dat een aanvaller gebruikmaakt van een gemanipuleerde identiteit kan niet worden geblokkeerd. 

Bij het instellen van het beleid, 

- Voorkomen dat gebruikers die niet / geen meerledige verificatie

- Gebruikers in landen waar het beleid inschakelen is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)

- Voorkomen dat gebruikers die kunnen genereren veel false-positieven (ontwikkelaars, analisten beveiliging)

- Gebruik een **hoge** drempel tijdens de eerste beleid zetten, of als u moet uitdagingen zichtbaar voor eindgebruikers minimaliseren.

- Een **lage** drempel gebruiken als uw organisatie meer beveiliging vereist. Selecteren van een **lage** drempel introduceert extra gebruiker aanmelden uitdagingen, maar verbeterde beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is een regel voor een **Gemiddeld** drempel een evenwicht tot stand tussen gebruiksgemak en beveiliging configureren.

 
Het beleid voor risico-in is:

- Toegepast op alle browserverkeer en aanmeldingen met moderne verificatie.
- Niet toegepast op toepassingen die gebruikmaken van oudere beveiligingsprotocollen door het eindpunt van de WS-Trust bij de federatieve IDP, zoals AD FS uit te schakelen.

Op de pagina **Risico's** in de console voor bescherming van identiteiten worden alle gebeurtenissen:

- Dit beleid is toegepast op
- U kunt de activiteit te controleren en bepalen of de actie nodig is 

Zie voor een overzicht van de bijbehorende gebruikerservaring:

- [Riskante recovery-in](active-directory-identityprotection-flows.md#risky-sign-in-recovery) 

- [Riskante aanmelden geblokkeerd](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  

- [Registratie meerledige verificatie tijdens een riskante aanmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)  





**Het bijbehorende dialoogvenster openen**:

1. Klik op het blad **identiteitsbeveiliging Azure AD** in de sectie **configureren** **- in risico-beleid**.

    ![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1014.png "Gebruikersbeleid voor ridk")





## <a name="multi-factor-authentication-registration-policy"></a>Registratiebeleid meerledige verificatie

Azure meerledige verificatie is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede laag van beveiliging voor aanmeldingen van gebruikers en transacties.  
We raden u aan Azure meerledige verificatie voor aanmeldingen van gebruikers omdat het:

- Sterke verificatie met tal van opties voor eenvoudige verificatie levert

- Speelt een belangrijke rol bij het opstellen van uw organisatie beveiligen en herstellen van compromissen account

![Gebruikersbeleid voor ridk] (./media/active-directory-identityprotection/1019.png "Gebruikersbeleid voor ridk")



Zie voor meer informatie [Wat is Azure meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)


Azure AD identiteitsbeveiliging helpt u bij het uitrollen van registratie meerledige verificatie door een beleid waarmee u kunt configureren: 



- Stel de gebruikers en groepen die het beleid is van toepassing op: 

    ![Beleid van de MVR gesloten] (./media/active-directory-identityprotection/1020.png "Beleid van de MVR gesloten")



- De besturingselementen worden afgedwongen wanneer het beleid wordt ingesteld:  

    ![Beleid van de MVR gesloten] (./media/active-directory-identityprotection/1021.png "Beleid van de MVR gesloten")


- De status van uw beleid gaan:

    ![Beleid van de MVR gesloten] (./media/active-directory-identityprotection/403.png "Beleid van de MVR gesloten")

- De status van de huidige registratie weergeven: 

    ![Beleid van de MVR gesloten] (./media/active-directory-identityprotection/1022.png "Beleid van de MVR gesloten")


Zie voor een overzicht van de bijbehorende gebruikerservaring:

- [Meerledige verificatie inschrijving stroom](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  

- [Registratie meerledige verificatie tijdens een riskante aanmelden](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).  





**Het bijbehorende dialoogvenster openen**:

1. Klik op het blad **identiteitsbeveiliging Azure AD** in de sectie **configureren** **registratie meerledige verificatie**.

    ![Beleid van de MVR gesloten] (./media/active-directory-identityprotection/1019.png "Beleid van de MVR gesloten")





## <a name="next-steps"></a>Volgende stappen

 - [Channel 9: Azure AD en identiteit weergeven: identiteit bescherming Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)
 - [Soorten risico's door Azure Active Directory identiteitsbeveiliging gedetecteerd](active-directory-identityprotection-risk-events-types.md)
 - [Beveiligingsproblemen ontdekt door Azure Active Directory identiteit bescherming](active-directory-identityprotection-vulnerabilities.md)
 - [Azure Active Directory identiteitsbeveiliging meldingen](active-directory-identityprotection-notifications.md)
 - [Azure Active Directory identiteitsbeveiliging playbook](active-directory-identityprotection-playbook.md)
 - [Azure Active Directory identiteitsbeveiliging woordenlijst](active-directory-identityprotection-glossary.md)

 - [-In ervaringen met Azure AD identiteit bescherming](active-directory-identityprotection-flows.md)

 - [Identiteitsbeveiliging Azure Active Directory inschakelen](active-directory-identityprotection-enable.md)
 - [Azure Active Directory identiteitsbeveiliging - blokkering opheffen van gebruikers](active-directory-identityprotection-unblock-howto.md)

 - [Aan de slag met Azure Active Directory identiteit bescherming en Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)



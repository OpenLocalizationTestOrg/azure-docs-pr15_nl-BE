<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - definiëren een hybride identiteit aangenomen strategie | Microsoft Azure"
    description="Azure Active Directory gecontroleerd met voorwaardelijke toegang beheren, de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en voor het toestaan van toegang tot de toepassing. Zodra deze voorwaarden wordt voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Een hybride identiteit aangenomen strategie definiëren

In deze taak, kunt u de hybride identiteit aangenomen strategie voor uw identiteit hybride oplossing om te voldoen aan de eisen die zijn beschreven in definiëren:

- [Zakelijke voorwaarden bepalen](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Vereisten voor directory-synchronisatie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Meerledige verificatie eisen bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Zakelijke behoefte bepalen
De eerste taak adressen bepalen van de organisaties zakelijke behoeften.  Dit kan zeer brede en reikwijdte kruip kan optreden als u niet voorzichtig bent.  Houd het simpel maar vergeet voor het plannen van een ontwerp dat wordt aangepast aan en vergemakkelijken van verandering in de toekomst in het begin.  Ongeacht of het een eenvoudig ontwerp of een uiterst complexe, is Azure Active Directory het platform Microsoft Identity die Office 365, Microsoft Online Services en cloud toepassingen ondersteunt.

## <a name="define-an-integration-strategy"></a>Definiëren van een Integratiestrategie voor
Microsoft heeft drie scenario's belangrijkste integratie die cloud identiteiten, gesynchroniseerde identiteiten en federatieve identiteiten.  Plan op te nemen van een van deze integratiestrategieën.  De strategie die u kan variëren en de beslissingen bij het kiezen van een kan bevatten, wat voor soort gebruikerservaring die u ter beschikking stellen wilt, hebt u enkele van de bestaande infrastructuur reeds ter plekke en wat is de meest voordelige kiezen.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

De scenario's die zijn gedefinieerd in de bovenstaande afbeelding zijn:

- **Wolk identiteiten**: deze identiteiten die uitsluitend in de cloud aanwezig zijn.  In het geval van Azure AD, zou ze zich bevinden in het bijzonder in de map Azure AD.
- **Gesynchroniseerde**: deze identiteiten die op het bedrijf aanwezig zijn en in de cloud.  Azure AD verbinding maken met, worden deze gebruikers gemaakt of samengevoegd met bestaande Azure AD accounts.  Hash van het wachtwoord van de gebruiker gesynchroniseerd van het milieu op het bedrijf naar de cloud in wat een hash van het wachtwoord heet.  Wanneer met behulp van gesynchroniseerd is de enige belemmering als een gebruiker in de omgeving van ruimten is uitgeschakeld, kan het maximaal 3 uur voor die accountstatus in Azure AD wilt laten duren.  Dit komt door het synchronisatie-interval.
- **Federatieve**: deze identiteiten zijn beide ruimten op en in de cloud.  Azure AD verbinding maken met, worden deze gebruikers gemaakt of samengevoegd met bestaande Azure AD accounts.  
 
>[AZURE.NOTE]
Voor meer informatie over de synchronisatie Lees opties [uw identiteiten op ruimten met Azure Active Directory integreren](active-directory-aadconnect.md).

De volgende tabel helpt bij het bepalen van de voor- en nadelen van elk van de volgende strategieën:

| Strategie         | Voordelen                                                                                                                                                                                                                                                  | Nadelen                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Wolk identiteiten** | Gemakkelijker te beheren voor kleine organisatie. <br> Niets te installeren op de lokalen geen extra hardware nodig<br>Eenvoudig uitgeschakeld als de gebruiker het bedrijf verlaat                                                                                                   | Gebruikers moeten aanmelden bij het benaderen van de werkbelasting in de cloud <br> Wachtwoorden al kunnen dan niet dezelfde voor identiteiten wolk en op locatie                                                                                                                                                                                                                     |
| **Gesynchroniseerd**     | Wachtwoord op ruimten wordt beide gebouwen op verifiëren en mappen van de wolk <br>Gemakkelijker te beheren voor kleine, middelgrote of grote organisaties <br>Gebruikers kunnen eenmalige aanmelding (SSO) hebben voor bepaalde resources <br> Microsoft preferred methode voor synchronisatie <br> Gemakkelijker te beheren | Sommige gebruikers mogelijk hun mappen synchroniseren met de cloud verschuldigde specifiek bedrijf politie terughoudend                                                                                                                                                                                                                                                  |
| **Federatieve**        | Eenmalige aanmelding (SSO) hebben gebruikers <br>Als een gebruiker wordt beëindigd of verlaat, de account onmiddellijk kan worden uitgeschakeld en toegang ingetrokken,<br> Ondersteuning biedt voor geavanceerde scenario's die niet kunnen worden bereikt met gesynchroniseerd                                           | Meer stappen voor het instellen en configureren <br> Hogere onderhoud <br> Extra hardware nodig voor de STS-infrastructuur <br> Extra hardware te installeren, de federation-server nodig. Extra software is nodig als AD FS wordt gebruikt. <br> Uitgebreide instellingen voor eenmalige aanmelding vereist <br> Kritieke punt van de fout als de federation-server is niet beschikbaar, gebruikers kunnen niet worden geverifieerd |

### <a name="client-experience"></a>Clientervaring
De strategie die u gebruikt, bepalen de gebruikerservaring-in.  De volgende tabellen bieden informatie over wat de gebruikers hun ervaring-in te verwachten.  Houd er rekening mee dat niet alle providers van federatieve id eenmalige aanmelding in alle scenario's ondersteunen.

**Deel uitmaakt van een domein en private network '-toepassingen**:
 

|                              | Gesynchroniseerde identiteit      | Federatieve identiteiten                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowsers                 | Op formulieren gebaseerde verificatie | eenmalige, soms vereist voor de levering van de organisatie-ID |
| Outlook                      | Vragen om referenties     | Vragen om referenties                                       |
| Skype voor bedrijven (Lync)    | Vragen om referenties     | eenmalige aanmelding voor Lync, referenties gevraagd voor Exchange   |
| SkyDrive Pro                 | Vragen om referenties     | eenmalige aanmelding                                               |
| Office Pro Plus abonnement | Vragen om referenties     | eenmalige aanmelding                                               |

**Externe of niet-vertrouwde bronnen**:

|                              | Gesynchroniseerde identiteit      | Federatieve identiteiten                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webbrowsers                 | Op formulieren gebaseerde verificatie |  Op formulieren gebaseerde verificatie |
| Outlook, Skype voor bedrijven (Lync), Skydrive Pro, Office-abonnement| Vragen om referenties     | Vragen om referenties                                       |
| Exchange ActiveSync    | Vragen om referenties     | eenmalige aanmelding voor Lync, referenties gevraagd voor Exchange   |
| Mobiele toepassingen                 | Vragen om referenties     | Vragen om referenties                                               |

Als u hebt bepaald van taak 1 dat u hebt een 3de partij IdP of zijn gaan als u wilt gebruiken een federatie met Azure Active Directory, moet u zich bewust zijn van de volgende ondersteunde mogelijkheden:
- Een provider van SAML 2.0 die voor het profiel van de SP-Lite compatibel is met verificatie van AD Azure kan ondersteunen en toepassingen die zijn gekoppeld
- Ondersteunt passieve verificatie wat auth OWA, gesimuleerde Productieorder, enz vergemakkelijkt.
- Online Exchange-clients kunnen worden ondersteund via de SAML 2.0 Enhanced Client profiel (ECP)

Ook moet u weten welke mogelijkheden er zijn niet meer beschikbaar:

- Zonder ondersteuning voor WS-Trust/Federation, wordt alle actieve clients verbroken.
 - Dat betekent dat er geen Lync client, OneDrive client, abonnement op Office, Office Mobile voor Office 2016
- Overgang van Office naar passieve verificatie kunnen zij zuivere IdPs van SAML 2.0 ondersteunen, maar ondersteuning blijft op basis van door de cliënt


>[AZURE.NOTE]
Lees het artikel http://aka.ms/ssoproviders voor de meest recente lijst.

## <a name="define-synchronization-strategy"></a>Strategie voor synchronisatie definiëren
In deze taak u de hulpprogramma's die worden gebruikt definieert voor het synchroniseren van de organisatie op-premises gegevens naar de cloud en wat moet u topologie.  Omdat de meeste organisaties wordt Active Directory gebruikt, vindt u informatie over het gebruik van Azure AD verbinden om de bovenstaande vragen in enkele details.  Voor omgevingen waarin Active Directory niet hebt, vindt u informatie over het met behulp van FIM 2010 R2 of MIM 2016 deze strategie.  Echter, toekomstige releases van Azure AD Connect biedt ondersteuning voor LDAP-adreslijsten, dus afhankelijk van de tijdlijn, deze informatie mogelijk helpen.

###<a name="synchronization-tools"></a>Hulpprogramma's voor synchronisatie
In de jaren hebben verschillende hulpprogramma's voor synchronisatie bestaat en die wordt gebruikt voor verschillende scenario's.  Azure AD verbinden is momenteel het Ga naar het hulpmiddel bij uitstek voor alle ondersteunde scenario's.  AAD Sync DirSync zijn ook nog steeds rond en kan zelfs aanwezig zijn in uw omgeving nu. 

>[AZURE.NOTE]
Voor de meest recente informatie over de ondersteunde mogelijkheden van elk hulpprogramma [Directory integration tools vergelijking](active-directory-hybrid-identity-design-considerations-tools-comparison.md) artikel te lezen.  

### <a name="supported-topologies"></a>Ondersteunde topologieën
Bij het definiëren van een strategie voor synchronisatie, moet de topologie die wordt gebruikt worden bepaald. Afhankelijk van de informatie die is vastgesteld in stap kunt 2 u bepalen welke topologie is het hulpmiddel te gebruiken De één forest één Azure AD-topologie is de meest voorkomende en bestaat uit één Active Directory-forest en een exemplaar van AD Azure.  Dit zal worden gebruikt in een meerderheid van de scenario's en de verwachte topologie is bij het gebruik van Azure AD verbinding Express-installatie zoals wordt weergegeven in de onderstaande afbeelding.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Scenario één Forest is vaak groot en zelfs kleine organisaties met meerdere forests, zoals afgebeeld in figuur 5.

>[AZURE.NOTE]
Voor meer informatie over de verschillende op gebouwen en Azure AD topologieën met Azure AD Connect lezen sync artikel [topologieën voor Azure AD verbinden](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Met meerdere forests

Als dit het geval is de topologie voor meerdere-forest-enkel Azure AD moet worden beschouwd als de volgende punten van toepassing zijn:

- Gebruikers hebben alleen 1 identiteit in alle forests – de unieke id gebruikers hieronder beschrijft dit in meer detail.
- De gebruiker wordt geverifieerd op het forest waarin zich de identiteit bevindt
- Bron anker (onveranderbare-id) en UPN-vandaan dit forest
- Alle forests zijn toegankelijk voor Azure AD verbinden – dit betekent hoeft niet domein gekoppeld en kan worden geplaatst in een DMZ als dit dit vergemakkelijkt.
- Gebruikers hebben alleen een Postvak
- Het forest dat als host fungeert voor de postbus van de gebruiker heeft de beste kwaliteit van de gegevens voor kenmerken die zichtbaar is in de Exchange algemene adreslijst (GAL)
- Als er geen postvak van de gebruiker, kan vervolgens een forest worden gebruikt om bij te dragen deze waarden
- Als u een gekoppelde postbus hebt, is ook een andere account in een ander forest gebruikt voor aanmelding.

>[AZURE.NOTE]
Objecten die in beide on-premises en in de cloud voorkomen zijn "verbonden" via een unieke id. In het kader van adreslijstsynchronisatie, deze unieke id het SourceAnchor genoemd. In de context van eenmalige aanmelding is dit het ImmutableId genoemd. [Ontwerp-concepten voor Azure AD verbinden](active-directory-aadconnect-design-concepts.md#sourceanchor) voor meer overwegingen met betrekking tot het gebruik van SourceAnchor.

Als het bovenstaande niet van toepassing zijn en er meer dan één actieve account of meer dan één postvak, wordt Azure AD verbinden een pick en negeren de andere.  Als u postbussen, maar geen andere account hebt gekoppeld, worden deze accounts niet geëxporteerd naar Azure AD en die gebruiker niet een lid van een groep.  Dit is anders dan hoe het was in het verleden met DirSync en opzettelijk aan betere ondersteuning voor deze situaties met meerdere forests. Een scenario met meerdere forests wordt weergegeven in de onderstaande afbeelding.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Met meerdere forests meerdere AD Azure scenario**

Wordt ondersteund, maar het wordt aanbevolen om één map Azure advertentie voor een organisatie het een 1:1 relatie tussen een sync Azure AD verbinding maken met server en een directory Azure AD blijft.  Voor elk exemplaar van AD Azure moet u een installatie van Azure AD verbinden.  Ook AD Azure, in het ontwerp is geïsoleerd en gebruikers in een exemplaar van AD Azure niet kunnen gebruikers in een ander exemplaar te zien.

Het is mogelijk en verbinding maken met één exemplaar van de in de lokalen van Active Directory meerdere AD Azure mappen zoals in de onderstaande afbeelding wordt ondersteund:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Filteren scenario één forest**

Hiervoor het volgende moet worden voldaan:

- Azure AD verbinden synchronisatie servers moeten worden geconfigureerd voor het filteren, zodat ze allebei een verzameling objecten die elkaar wederzijds uitsluiten hebben.  Dit gedaan, bijvoorbeeld door het bereik van elke server op een bepaald domein of organisatie-eenheid.
- Een DNS-domein, kan alleen worden geregistreerd in één directory Azure AD zodat de UPN-namen van de gebruikers in de lokalen op AD aparte naamruimten moet gebruiken
- Alleen is gebruikers in één exemplaar van AD Azure kunnen gebruikers hun exemplaar te zien.  Ze worden niet Zie gebruikers in de andere gevallen.
- Alleen een van de adreslijsten Azure AD Exchange hybride met op ruimten kunt inschakelen AD
- Wederzijdse exclusiviteit ook van toepassing op terugschrijven.  Hierdoor worden sommige write-back-functies niet ondersteund met deze topologie, omdat deze wordt uitgegaan van een configuratie met één op gebouwen.  Dit omvat:
 - Terugneming groeperen met standaardconfiguratie
 - Terugneming van apparaat


Let erop dat het volgende wordt niet ondersteund en moet niet worden gekozen als een van de implementatie:

- Het wordt niet ondersteund als u meerdere servers met Azure AD verbinden sync verbinding maken met dezelfde map Azure AD, zelfs als ze zijn geconfigureerd voor het synchroniseren van elkaar uitsluitende set van object
- Het wordt niet ondersteund als u wilt synchroniseren dezelfde gebruiker tot meerdere directory's van Azure AD. 
- Het wordt ook niet ondersteund als u een configuratie wijzigen zodat gebruikers een Azure advertentie weergegeven als contactpersonen in de map van een ander AD Azure. 
- Het is ook niet-ondersteunde sync Azure AD verbinden verbinding maken met meerdere AD Azure mappen wijzigen.
- Azure AD mappen zijn inherent geïsoleerd. Het wordt niet ondersteund voor de configuratie van Azure AD verbinden synchroniseren met gegevens uit een andere map met Azure AD lezen in een poging tot het maken van een gemeenschappelijke en uniforme GAL tussen de mappen wijzigen. Wordt deze niet ondersteund gebruikers wilt exporteren als contactpersonen naar een andere op-premises Azure AD verbinden synchroniseren met AD.


>[AZURE.NOTE]
Als uw organisatie ervoor zorgt dat computers in het netwerk verbinding met het Internet, in dit artikel worden de eindpunten (FQDN-namen, IPv4 en IPv6-adres bereiken) moet u opnemen in uw uitgaande lijsten en Internet Explorer de Zone Vertrouwde websites van client computers toestaan om te zorgen dat uw computers kunnen is Office 365. Lees voor meer informatie de [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Meerledige verificatie bepalen
In deze taak definieert u de strategie meerledige verificatie te gebruiken.  Azure meerledige verificatie wordt geleverd in twee verschillende versie.  Een cloud-gebaseerde is en de andere op basis van de MVR gesloten Azure server op locatie.  Op basis van de evaluatie u kunt u bepalen welke oplossing de juiste is voor uw strategie is.  Gebruik de onderstaande tabel om te bepalen welke optie ontwerp beste te voldoen aan de beveiligingseisen van uw bedrijf:

Meerledige ontwerpopties:

| Activa te beveiligen                                               | MVR gesloten in de cloud | MVR gesloten lokale |
|---------------------------------------------------------------|------------------|----------------|
| Microsoft-toepassingen                                                | Ja              | Ja            |
| SaaS-apps in de app-galerie                                  | Ja              | Ja            |
| IIS-toepassingen wordt gepubliceerd via Azure AD App Proxy         | Ja              | Ja            |
| IIS-toepassingen die niet zijn gepubliceerd via de Proxy Azure AD App | geen               | Ja            |
| RAS als VPN, RDG                                     | geen               | Ja            |

Hoewel u mogelijk hebt gekozen voor een oplossing voor uw strategie, moet u nog steeds gebruik van de evaluatie van boven op waar de gebruikers zich bevinden.  Hierdoor kan de oplossing te wijzigen.  Gebruik de onderstaande tabel helpt u om dit te bepalen:

| Locatie                                                       | Optie voorkeur ontwerp                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Multi-FactorAuthentication in de cloud |
| Azure AD en Federatie met AD FS met AD ruimten             | Beide                                    |
| Azure AD en in ruimten met Azure AD AD geen sync wachtwoord verbinding | Beide                                    |
| Azure AD en in ruimten met Azure AD verbinden met wachtwoord synchronisatie  | Beide                                    |
| AD ruimten                                                      | Meerledige verificatieserver      |

>[AZURE.NOTE]
U moet ook ervoor zorgen dat de optie meerledige verificatie ontwerp dat u hebt geselecteerd ondersteunt de functies die vereist voor uw ontwerp zijn.  Lees [de meerledige beveiligingsoplossing voor u kiezen](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure)voor meer informatie.

## <a name="multi-factor-auth-provider"></a>Meerledige verificatie Provider
Meerledige verificatie is standaard beschikbaar voor globale beheerders met een huurder Azure Active Directory. Echter, als u wilt verlengen meerledige verificatie voor alle gebruikers en/of uw globale beheerders kunnen nemen voordeel functies zoals het management, portal, aangepaste begroetingen en rapporten wilt vervolgens moet kopen en meerledige verificatie configureren.

>[AZURE.NOTE]
U moet ook ervoor zorgen dat de optie meerledige verificatie ontwerp dat u hebt geselecteerd ondersteunt de functies die vereist voor uw ontwerp zijn. 

##<a name="next-steps"></a>Volgende stappen
[Bepalen van vereisten voor de bescherming van gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)

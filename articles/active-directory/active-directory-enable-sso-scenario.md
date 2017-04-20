<properties
    pageTitle="Beheren van toepassingen met Azure Active Directory | Microsoft Azure"
    description="Dit artikel de voordelen van Azure Active Directory integreren met op lokalen, cloud en SaaS-toepassingen."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Toepassingen met Azure Active Directory beheren

Bedrijven hebben dan de daadwerkelijke werkstroom of inhoud twee basisvereisten voor alle toepassingen:

1. Om de productiviteit te verhogen, moeten toepassingen gemakkelijk vinden en benaderen

2. Als u beveiligings- en governance, moet de organisatie controle en toezicht op wie kan en daadwerkelijk toegang heeft tot elke toepassing

In de wereld van cloud toepassingen kan dit best worden bereikt met identiteit bepalen '*WIE mag wat doen*'.

Bij de berekening van terminologie:

- *Wie* staat bekend als *identiteit* - het beheer van gebruikers en groepen

- *Wat* is bekend als *access management* – het beheer van de toegang tot beveiligde bronnen

Beide componenten bij elkaar worden genoemd *Identity and Access Management (IAM)*, die wordt gedefinieerd door de groep [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) als '*de discipline van beveiliging waarmee u de juiste personen toegang te krijgen tot de juiste resources aan de rechterkant tijden voor de redenen waarom*".

OK, dus wat is het probleem? Als IAM *niet beheerd* op één locatie met een geïntegreerde oplossing:

- Identiteit-beheerders hebben afzonderlijk maken en bijwerken van de gebruikersaccounts in alle toepassingen afzonderlijk een redundante en tijdrovende activiteit.

- Gebruikers hebben tot meerdere referenties voor toegang tot de toepassingen die ze nodig hebben om te werken met onthouden. Hierdoor kunnen vaak gebruikers schrijf hun wachtwoorden of andere oplossingen voor wachtwoord dat andere gegevens veiligheidsrisico's gebruiken.

- Redundante, tijdrovende activiteiten verminderen het bedrag van de gebruikers en beheerders werkt op zakelijke activiteiten die de winstgevendheid van uw bedrijf verhogen.

Dus wat in het algemeen voorkomt dat organisaties van geïntegreerde IAM oplossingen vast te stellen?

- Meest technische oplossingen zijn gebaseerd op platforms voor software die moeten worden geïmplementeerd en aangepast door elke organisatie voor hun eigen toepassingen.

- Cloud-toepassingen worden vaak aangenomen met een hogere snelheid dan de IT-organisatie kunt integreren met bestaande IAM oplossingen.

- Beveiliging en bewaking van tooling nodig extra aanpassing en integratie te bereiken, uitgebreide E2E-scenario's.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory geïntegreerd met toepassingen

Azure Active Directory is de uitgebreide identiteit als (IDaaS) serviceoplossing van Microsoft die:

- IAM kunnen als een cloud-service 

- Biedt een centrale toegangsbeheer, eenmalige aanmelding (SSO) en rapportage 

- Ondersteunt toegangsbeheer voor [duizenden toepassingen](https://azure.microsoft.com/marketplace/active-directory/) in de galerie van toepassing, met inbegrip van televergaderingen, Google Apps, vak Concur en meer geïntegreerd. 


Met Azure Active Directory, alle toepassingen die u publiceert uw partners en klanten (business of consument) hebben dezelfde identiteit en toegang tot de beheerfuncties.<br> Hiermee kunt u uw operationele kosten aanzienlijk verminderen.

Wat gebeurt er als u nodig voor de uitvoering van een toepassing die nog niet wordt weergegeven in de galerie van toepassing? Dit is een beetje meer tijd in beslag dan eenmalige aanmelding configureren voor toepassingen in de galerie toepassing, biedt Azure AD u een wizard die u met de configuratie helpt.

De waarde van advertentie Azure gaat verder dan "alleen" cloud-toepassingen. Ook kunt u deze met toepassingen voor bedrijven via een beveiligde externe toegang. Met veilige externe toegang u kunt voorkomen dat het de noodzaak voor VPN-verbindingen of andere implementaties van traditionele RAS-beheer.

Azure AD biedt centraal toegangsbeheer en eenmalige aanmelding (SSO) bieden voor al uw toepassingen, de oplossing voor problemen met de belangrijkste gegevens beveiliging en productiviteit.

- Gebruikers kunnen toegang krijgen tot meerdere toepassingen met één teken op meer tijd geven om inkomsten te genereren of zakelijke operationele activiteiten gedaan.

- Identiteit-beheerders kunnen toegang tot toepassingen op één plaats beheren.

Het voordeel voor de gebruiker en voor uw bedrijf is duidelijk. Laten we dichter op de voordelen voor de beheerder van de identiteit en de organisatie.

## <a name="integrated-application-benefits"></a>Voordelen van geïntegreerde toepassing

Het proces voor eenmalige aanmelding heeft twee stappen:

- Verificatie, het proces van de identiteit van de gebruiker.

- Toelating, het besluit tot inschakelen of blokkeren toegang tot een bron met een-beleid.

Bij gebruik van Azure AD toepassingen beheren en eenmalige aanmelding inschakelen:

- Verificatie wordt uitgevoerd op (bijvoorbeeld AD) ruimten of Azure AD-account van de gebruiker.

- Vergunning op de Azure AD toewijzings- en beleid ervoor te zorgen dat eindgebruikers consistent en zodat u de toewijzing, de locaties en de voorwaarden van de MVR gesloten op elke toepassing, ongeacht de interne mogelijkheden toevoegen wordt uitgevoerd.

Het belangrijk te begrijpen dat de manier waarop de vergunning van op de doeltoepassing kracht wordt varieert afhankelijk van de wijze waarop de toepassing is geïntegreerd met AD Azure.

- **Toepassingen vooraf geïntegreerde service provider** Zoals Office 365 en Azure zijn deze toepassingen rechtstreeks op Azure AD ingebouwd en afhankelijk van de uitgebreide mogelijkheden voor identiteits- en toegangsbeheer. Toegang tot deze toepassingen is ingeschakeld via de directory-gegevens als het token uitgiftebeleid.

- **Toepassingen vooraf geïntegreerde door Microsoft en aangepaste toepassingen** Dit zijn onafhankelijke cloud-toepassingen die afhankelijk zijn van een interne toepassingsmap en kunnen werken onafhankelijk van Azure AD. Toegang tot deze toepassingen is door afgifte van een toepassing specifieke referentie toegewezen aan de account van een toepassing ingeschakeld. Afhankelijk van de toepassingsmogelijkheden, de referentie mogelijk een token van de federation of gebruikersnaam en wachtwoord voor een account die eerder werd ingericht in de toepassing.

- **On-premises toepassingen** Toepassingen worden gepubliceerd via de Azure AD toepassingsproxy voornamelijk toegang geven tot toepassingen voor gebouwen. Deze toepassingen zijn gebaseerd op een centraal in de lokale directory, zoals Windows Server Active Directory. Toegang tot deze toepassingen is ingeschakeld door het activeren van de proxy voor het leveren van de toepassingsinhoud aan de eindgebruiker tijdens de eis van de aanmelding op gebouwen behouden blijft.

Bijvoorbeeld als een gebruiker lid wordt van uw organisatie, moet u een account voor de gebruiker maken in Azure AD voor de bewerkingen voor primaire. Als deze gebruiker heeft toegang nodig tot een beheerde toepassing zoals televergaderingen, moet u ook een account voor deze gebruiker in televergaderingen maken en koppelen aan de account Azure te maken van eenmalige aanmelding, werkt. Wanneer de gebruiker uw organisatie verlaat, is het raadzaam de Azure AD-account te verwijderen en van de toepassingen die de gebruiker heeft toegang tot alle rekeningen van de tegenwaarde in de IAM wordt opgeslagen.

## <a name="access-detection"></a>Detectie van Access

In moderne ondernemingen, IT-afdelingen vaak niet op de hoogte zijn van alle cloud-toepassingen die worden gebruikt. In combinatie met de Cloud App Discovery biedt Azure AD een oplossing voor deze toepassingen detecteren.

## <a name="account-management"></a>Accountbeheer

Traditioneel beheren van accounts in de diverse toepassingen is een handmatig proces uitgevoerd door IT of in de organisatie van persoonlijke ondersteuning. Accountbeheer Azure AD volledig geautomatiseerd in alle toepassingen van serviceprovider geïntegreerd en die vooraf door Microsoft ondersteuning voor Automatische gebruikersaanvragen of SAML JIT geïntegreerde toepassingen.

## <a name="automated-user-provisioning"></a>Geautomatiseerde gebruikersaanvragen

Sommige toepassingen bieden interfaces van automatisering voor het maken en verwijderen of deactiveren van accounts. Als een provider een interface biedt, wordt deze overgenomen door AD Azure. Hierdoor vermindert uw operationele kosten omdat administratieve taken automatisch worden uitgevoerd en verbetert de beveiliging van uw omgeving omdat dit de kans op onbevoegde toegang verkleint.

## <a name="access-management"></a>Toegangsbeheer

Met Azure AD kunt u toegang tot toepassingen met behulp van afzonderlijke of regel aangedreven toewijzingen beheren. U kunt ook overdragen toegang beheer om de juiste mensen in de organisatie van het toezicht op de beste zorgen en het verminderen van de belasting van de helpdesk.

## <a name="on-premises-applications"></a>Toepassingen in gebouwen

De ingebouwde in application proxy kunt u voor het publiceren van uw toepassingen in gebouwen waardoor gebruikers zowel consistente ervaring met de toepassing van moderne cloud en de voordelen van de bewaking, rapportage en beveiliging mogelijkheden Azure AD toegang.

## <a name="reporting-and-monitoring"></a>Rapportage en monitoring van

AD Azure biedt vooraf geïntegreerde rapportage en monitoring van de mogelijkheden die het mogelijk maken om te weten wie heeft toegang tot toepassingen en ze daadwerkelijk gebruikt.

## <a name="related-capabilities"></a>Verwante functies

U kunt uw toepassingen met gedetailleerde-beleid en vooraf geïntegreerde MVR gesloten beveiligen met Azure Active Directory. Voor meer dat informatie over Azure MVR gesloten Zie [Azure MVR gesloten](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Aan de slag

Om te beginnen toepassingen integreren met AD Azure, kijk eens naar de [integratie van Azure Active Directory met toepassingen die aan de slag](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Zie ook

[Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

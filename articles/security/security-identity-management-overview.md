<properties
   pageTitle="Azure identity management beveiliging-overzicht | Microsoft Azure"
   description=" Microsoft identity en access management oplossingen zorgen ervoor dat IT beschermen toegang tot toepassingen en bronnen binnen het computercentrum en in de cloud, waardoor extra niveaus van validatie van de meerledige verificatie en beleid voor voorwaardelijke toegang. Dit artikel bevat een overzicht van de belangrijkste Azure beveiligingsfuncties die bij het identiteitsbeheer helpen. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Azure identity management beveiliging-overzicht

Microsoft identity en access management oplossingen zorgen ervoor dat IT beschermen toegang tot toepassingen en bronnen binnen het computercentrum en in de cloud, waardoor extra niveaus van validatie van de meerledige verificatie en beleid voor voorwaardelijke toegang. Verdachte activiteiten controleren door middel van geavanceerde beveiliging, rapportage, controle en signalering helpt potentiële beveiligingsproblemen beperken. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) biedt eenmalige aanmelding met duizenden wolk apps (SaaS) en toegang tot web apps die u uitvoert op gebouwen.

Beveiliging voordelen van Azure Active Directory (AD) de mogelijkheid om:

- Maken en beheren van de identiteit van een enkel voor elke gebruiker binnen uw onderneming hybride, gebruikers, groepen en apparaten synchroon houden
- Enkele aanmelding toegang bieden tot uw toepassingen met inbegrip van duizenden vooraf geïntegreerde SaaS-apps
- Beveiliging van access-toepassingen door af te dwingen voor beide ruimten op regels gebaseerde meerledige verificatie inschakelen en cloud toepassingen
- Bepaling veilige externe toegang tot webtoepassingen voor bedrijven via Azure AD toepassingsproxy

Het doel van dit artikel is bedoeld als een overzicht van de belangrijkste Azure beveiligingsfuncties die bij het identiteitsbeheer helpen. Wij bieden ook koppelingen naar artikelen waarin de details van elke functie geeft zodat u meer informatie.  

Het artikel is gericht op de volgende core Azure voor identiteitsbeheer:

- Eenmalige aanmelding
- Reverse-proxy
- Meerledige verificatie
- Beveiliging bewaking, waarschuwingen en machine learning gebaseerde rapporten
- Consument identiteits- en management
- De registratie
- Beschermde identiteitsbeheer
- Bescherming van de identiteit
- Hybride identity management

## <a name="single-sign-on"></a>Eenmalige aanmelding

Enkele aanmelding (SSO) houdt toegang tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, door slechts eenmaal met behulp van één gebruikersaccount aanmelden. Eenmaal ingelogd, u hebt toegang tot alle toepassingen die u nodig hebt, zonder verificatie (bv. Typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een service (SaaS)-toepassingen zoals Office 365, vak- en televergaderingen voor de productiviteit van de eindgebruiker. In het verleden, IT-personeel nodig zijn afzonderlijk maken en bijwerken van de gebruikersaccounts in elke SaaS-applicatie en moesten gebruikers een wachtwoord voor elke SaaS-applicatie.

Azure AD breidt Active Directory op de ruimten in de cloud, zodat gebruikers kunnen met hun primaire organisatie-account kunt u niet alleen aanmelden bij hun apparaten domein behoren en bedrijf resources maar ook alle web en SaaS-toepassingen die nodig zijn voor hun taak.

Niet alleen gebruikers hoeft niet voor het beheren van meerdere sets met gebruikersnamen en wachtwoorden, toegang tot toepassingen kan automatisch worden ingericht of opgeheven ingericht op basis van groepen en hun status als werknemer. Azure AD introduceert beveiliging en toegangsbeheer governance waarmee u de toegang van gebruikers centraal beheren via SaaS-toepassingen.

Meer informatie:

- [Overzicht van Single Sign-On](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Eenmalige aanmelding Azure Active Directory is geïntegreerd met SaaS apps](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Reverse-proxy

Azure AD toepassingsproxy kunt u publiceren op gebouwen-toepassingen zoals [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) -sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)en [IIS](http://www.iis.net/)-toepassingen binnen uw particuliere netwerk gebaseerd en beveiligde toegang biedt aan gebruikers buiten uw netwerk. Toepassingsproxy biedt externe toegang en eenmalige aanmelding (SSO) voor veel soorten webtoepassingen voor bedrijven met de duizenden SaaS-toepassingen die Azure AD ondersteunt. Werknemers kunnen zich aanmelden bij uw apps uit op hun eigen apparaten thuis en via deze proxy cloud-gebaseerde verificatie.

Meer informatie:

- [Toepassingsproxy Azure AD inschakelen](../active-directory/active-directory-application-proxy-enable.md)
- [Uitgeven van toepassingen met behulp van AD-toepassingsproxy Azure](../active-directory/active-directory-application-proxy-publish.md)
- [Single-sign-on met de toepassingsproxy](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Werken met voorwaardelijke toegang](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Meerledige verificatie
Azure meerledige verificatie (MVR gesloten) is een methode voor verificatie vereist het gebruik van meer dan één verificatiemethode en een kritische tweede beveiligingslaag toegevoegd op aanmeldingen van gebruikers en transacties. MVR gesloten helpt beschermen de toegang tot gegevens en toepassingen bij het voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Levert een sterke verificatie via een reeks opties voor verificatie, telefoongesprek, tekstbericht of mobiele app kennisgevings- of code en 3de partij OAuth-tokens.

Meer informatie:

- [Meerledige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Wat is Azure meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)
- [De werking van Azure meerledige verificatie](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiliging bewaking, waarschuwingen en machine learning gebaseerde rapporten

Beveiliging bewaking en waarschuwingen en machine learning gebaseerde rapporten over access inconsistente patronen kunt u uw bedrijf te beschermen. U kunt Azure Active Directory-toegang en gebruiksrapporten krijgen inzicht in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt een directory admin beter bepalen waar mogelijke beveiligingsrisico's kunnen liggen, zodat zij adequaat plannen kunnen die risico's te beperken.

Rapporten zijn gecategoriseerd in Azure klassieke portal op de volgende manieren:

- Afwijking rapporten – bevatten gebeurtenissen die we gevonden worden afwijkende inloggen. Ons doel is u op de hoogte van dergelijke activiteiten en u kunt bepalen of een gebeurtenis is verdacht.
- Geïntegreerde toepassing rapporten – bieden inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
- Foutenrapporten – geven fouten die kunnen optreden tijdens het inrichten van accounts voor externe toepassingen.
- Gebruiker-specifieke rapporten – weergeven apparaat/sign in activiteitsgegevens voor een specifieke gebruiker.
- Logboeken voor faxactiviteit – bevatten een overzicht van alle gecontroleerde gebeurtenissen binnen de afgelopen 24 uur, laatste 7 dagen of laatste 30 dagen, alsmede wijzigingen in activiteiten en wachtwoord opnieuw instellen en de registratie van de activiteit.

Meer informatie:

- [En het gebruik van access-rapporten weergeven](../active-directory/active-directory-view-access-usage-reports.md)
- [Aan de slag met Azure Active Directory Reporting](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory Reporting-handleiding](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Consument identiteits- en management

Azure Active Directory B2C is een hoge beschikbaarheid, wereldwijde, identiteit management-service voor de consument gerichte toepassingen die wordt aangepast aan de honderden miljoenen identiteiten. Het kan worden geïntegreerd in mobiele en web-platforms. De consumenten kunnen aanmelden bij uw toepassingen via aanpasbare ervaringen met hun bestaande sociale accounts of door het maken van nieuwe referenties.

In het verleden zouden toepassingsontwikkelaars die aanmelden en meld u bij de consumenten in hun toepassingen wilden hun eigen code hebt geschreven. En ze zou hebben gebruikt op ruimten databases of systemen voor het opslaan van gebruikersnamen en wachtwoorden. B2C van Azure Active Directory biedt uw organisatie een betere manier consument identiteitsbeheer integreren in toepassingen met behulp van een beveiligde, op standaards gebaseerd platform en een grote verzameling beleidsregels extensible.

Wanneer u Active Directory-B2C Azure, kunnen de consumenten aanmelden voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door het maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord).

Meer informatie:

- [Wat is Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C-voorbeeld: ondertekenen omhoog en meld je aan consumenten in uw toepassingen](../active-directory-b2c/active-directory-b2c-overview.md)
- [Voorbeeld van Azure Active Directory B2C: Typen toepassingen](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>De registratie

Azure AD de registratie is de basis voor scenario's op basis van het apparaat van [voorwaardelijke toegang](../active-directory/active-directory-conditional-access-on-premises-setup.md) . Wanneer een apparaat wordt geregistreerd, vindt Azure Active Directory de registratie u het apparaat met een identiteit die wordt gebruikt voor het verifiëren van het apparaat wanneer de gebruiker zich aanmeldt. De geverifieerde apparaat en de kenmerken van het apparaat, kunnen vervolgens gebruikt om af te dwingen van voorwaardelijke-beleidsregels voor toepassingen die worden gehost in de wolk en in ruimten.

In combinatie met een mobile device management (MDM) oplossing zoals Intune, worden de kenmerken van het apparaat in Azure Active Directory bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u voor het maken van regels voor voorwaardelijke toegang die toegang van apparaten om te voldoen aan de normen voor beveiliging en naleving afdwingen.

Meer informatie:

- [Aan de slag met Azure Active Directory de registratie](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Instellen van de voorwaardelijke toegang voor ruimten met Azure Active Directory de registratie](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automatische registratie met Azure Active Directory voor Windows-apparaten een domein behoren](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer
Azure Active Directory (AD) beschermde Identity Management kunt u beheren, besturen, en beschermde identiteiten en de toegang tot bronnen in Azure AD, evenals andere Microsoft online services, zoals Office 365 of Microsoft Intune.

Gebruikers moeten soms om de begunstigde verrichtingen in Azure of Office 365 resources of andere SaaS-toepassingen uit te voeren. Dit betekent vaak organisaties hebben ze in Azure AD permanente bevoorrechte toegang geven. Dit is een toenemend risico voor resources cloud gehost omdat organisaties voldoende kunnen niet controleren wat de gebruikers doen aan hun rechten admin. Ook als een gebruikersaccount met bevoorrechte toegang is aangetast, die een inbreuk kan van invloed zijn hun algehele beveiliging in de cloud. Azure rechten AD Identity Management helpt bij het oplossen van dit risico.

Azure rechten AD Identity Management kunt u:

- Zien welke gebruikers zijn beheerders Azure AD
- On-demand 'just in time"administratieve toegang tot Microsoft Online Services, zoals Office 365 en Intune inschakelen
- Rapporten over de geschiedenis van beheerder toegang en wijzigingen in toewijzingen beheerder ophalen
- Ontvang berichten over de toegang tot een beschermde rol

Meer informatie:

- [Azure AD bevoorrechte Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Rollen in Azure AD bevoorrecht Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD beschermde Identity Management: Het toevoegen of verwijderen van een gebruikersfunctie](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Bescherming van de identiteit
AD identiteitsbeveiliging Azure is een security service biedt een geconsolideerde weergave van risico's en potentiële beveiligingsproblemen op het gebied van de identiteit van uw organisatie. Bescherming van identiteiten maakt gebruik van bestaande Azure Active Directory de afwijking functies (beschikbaar via Azure AD afwijkende activiteitsrapporten) en introduceert nieuwe risico gebeurtenistypen die in real-time kunnen anomalieën.

Meer informatie:

- [Identiteitsbeveiliging Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Channel 9: Azure AD en identiteit weergeven: identiteit bescherming Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybride identity management

De aanpak van Microsoft naar identiteit omvat op gebouwen en de cloud, het maken van een enkele gebruikersidentiteit voor verificatie en machtiging tot alle bronnen, ongeacht de locatie.

Meer informatie:

- [Hybride identiteit-witboek](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory-teamblog](https://blogs.technet.microsoft.com/ad/)

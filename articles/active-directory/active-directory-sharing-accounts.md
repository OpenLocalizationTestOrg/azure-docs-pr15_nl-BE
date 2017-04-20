<properties
    pageTitle="Delen van accounts met Azure AD |  Microsoft Azure"
    description="Hierin wordt beschreven hoe Azure Active Directory kunnen organisaties rekeningen voor apps in de ruimten en de consument cloud services veilig te delen."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Delen van accounts met Azure Active Directory

## <a name="overview"></a>Overzicht
Organisaties moeten soms een enkele gebruikersnaam en wachtwoord gebruikt voor meerdere mensen. Dit gebeurt meestal in twee gevallen:

- Bij het openen van toepassingen die een unieke aanmeldings-id en wachtwoord voor elke gebruiker vereist of apps op bedrijven of consumenten cloud services (bv. zakelijke sociale media accounts).
- Bij het maken van omgevingen met meerdere gebruikers. Er kan een lokale account verreikende machtigingen zijn verbonden en kan worden gebruikt voor installatie, beheer en herstel activiteiten (bv. lokale "globale administrator'-account voor Office 365) of de root-account in de televergaderingen core.

Vroeger zou deze rekeningen worden gedeeld door de referenties (gebruikersnaam en wachtwoord) om de juiste personen te verspreiden of opslaan op een gedeelde locatie waar meerdere vertrouwde agenten er toegang toe hebben.

Het traditionele model delen heeft verschillende nadelen:

- Toegang tot nieuwe toepassingen moet u referenties voor iedereen die toegang moeten verdelen.
- Elke gedeelde toepassing mogelijk een eigen unieke set gedeelde referenties, zodat gebruikers meerdere sets met referenties te onthouden. Gebruikers hebben veel referenties onthouden, verhoogt het risico dat zij zal gebruik maken van risicovolle praktijken. (b.v. opschrijven van wachtwoorden).
- U kan niet bepalen wie toegang heeft tot een toepassing.
- U weet niet wie heeft *toegang tot* een toepassing.
- Wanneer u toegang tot een toepassing verwijdert moet, hebt u de referenties bijwerken en deze opnieuw te distribueren aan iedereen die toegang tot deze toepassing nodig heeft.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-account delen

AD Azure biedt een nieuwe aanpak op het gebruik van gedeelde accounts waarmee deze nadelen.

De beheerder AD Azure configureren welke toepassingen die de gebruiker toegang met behulp van het Configuratiescherm toegang en kiezen van het type single sign-on best geschikt is voor die toepassing. Een van die soorten, *op basis van wachtwoorden eenmalige aanmelding*kunt Azure AD fungeren als een soort "broker" tijdens de aanmelding voor die toepassing.

Gebruikers melden zich in één keer met de rekeningen van hun organisatie. Dit is dezelfde account die ze regelmatig gebruiken voor toegang tot hun desktop of e-mailadres. Ontdekken kunnen en de toegang tot alleen die toepassingen waaraan ze zijn toegewezen. Deze lijst van toepassingen kan een willekeurig aantal gedeelde referenties bevatten met gedeelde accounts. De gebruiker hoeft niet te onthouden of noteer de verschillende rekeningen die mogelijk wordt gebruikt.

Gedeelde accounts niet alleen toezicht te verhogen en verbeteren van de bruikbaarheid, ze ook beveiliging vergroten. Gebruikers met machtigingen voor het gebruik van de referenties gedeelde wachtwoorden niet ziet, maar in plaats daarvan machtigingen het wachtwoord gebruiken als onderdeel van een verificatie geregiseerde stroom krijgen. Met bepaalde toepassingen van de SSO-wachtwoord hebt u bovendien Azure AD laten regelmatig rollover (update) het wachtwoord met behulp van grote, complexe wachtwoorden verhogen de beveiliging van de rekening. De beheerder kan eenvoudig verlenen of intrekken van toegang tot een toepassing en ook weten wie toegang heeft tot de account en die toegang tot in het verleden.

Azure AD ondersteunt gedeelde accounts voor alle Enterprise Mobility Suite (EMS), Premium of Basic gelicentieerde gebruikers voor alle soorten één wachtwoord aanmelden op toepassingen. U kunt accounts voor duizenden vooraf geïntegreerde toepassingen in de galerie toepassing delen en uw eigen wachtwoord verificatie toepassing met [aangepaste SSO-apps](active-directory-sso-integrate-saas-apps.md)kunt toevoegen.

Azure zijn AD waarmee account delen:

- [Eenmalige aanmelding wachtwoord](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Wachtwoord single sign-on-agent
- [Toewijzingen groeperen](active-directory-accessmanagement-self-service-group-management.md)
- Aangepaste wachtwoord apps
- [App dashboard/rapporten](active-directory-passwords-get-insights.md)
- Gebruiker toegang tot portals
- [App-proxy](active-directory-application-proxy-get-started.md)
- [Active Directory-marktplaats](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Delen van een account
Azure AD voor het delen van een account die u wilt gebruiken:

- Een [aangepaste toepassing](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) of een toepassing [app galerie](https://azure.microsoft.com/marketplace/active-directory/) toevoegen
- De toepassing voor wachtwoord Single Sign On (SSO) configureren
- Gebruik [groep op basis van toewijzing](active-directory-accessmanagement-group-saasapps.md) en selecteer de optie een gedeelde referenties invoeren
- Optioneel: in sommige toepassingen, zoals Facebook, Twitter of LinkedIn, kunt u de optie inschakelen voor [Automatische wachtwoord Azure AD roll-over](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

U kunt ook uw gedeelde account beter beveiligen met MVR (gesloten meerledige-verificatie) (meer informatie over het [beveiligen van toepassingen met Azure Active Directory](../multi-factor-authentication/multi-factor-authentication-get-started.md)) en u de mogelijkheid om te beheren wie toegang heeft tot de toepassing met behulp van Groepsbeleidsbeheer [Azure AD zelf](active-directory-accessmanagement-self-service-group-management.md) kunt overdragen.

## <a name="related-articles"></a>Verwante artikelen

- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)
- [Bescherming van de apps met voorwaardelijke toegang](active-directory-conditional-access.md)
- [Self-service groep management/SSAA](active-directory-accessmanagement-self-service-group-management.md)

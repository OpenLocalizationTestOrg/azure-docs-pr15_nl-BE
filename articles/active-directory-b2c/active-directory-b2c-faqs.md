<properties
    pageTitle="Azure Active Directory B2C: Veelgestelde vragen | Microsoft Azure"
    description="Veelgestelde vragen over Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: veelgestelde vragen

Deze pagina antwoorden op veelgestelde vragen over de B2C Azure Active Directory (AD Azure). Regelmatig terug voor updates.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kan ik in mijn bestaande, op basis van een werknemer Azure AD huurder Azure AD B2C-functies gebruiken?

Momenteel kunnen niet Azure AD B2C-functies worden ingeschakeld in uw bestaande Azure AD huurder. Het is raadzaam een afzonderlijke huurder Azure AD B2C-functies gebruiken voor het beheren van de consumenten te maken.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kan ik Azure AD B2C gebruiken om sociale inloggen (Facebook en Google +) in Office 365?

Azure AD B2C kan niet worden gebruikt met Microsoft Office 365. Het kan niet in het algemeen worden gebruikt voor verificatie naar een SaaS-apps (Office 365, televergaderingen, werkdag, enz.). Het biedt identiteits-en alleen voor de consument gerichte web en mobiele toepassingen en is niet van toepassing op de werknemer of partner-scenario's.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Wat zijn lokale accounts in Azure AD B2C? Hoe verschillen zij van werk of school in Azure AD?

In een huurder Azure AD elke gebruiker in de huurder (met uitzondering van gebruikers met bestaande accounts van Microsoft) met een e-mailadres van het formulier ondertekent `<xyz>@<tenant domain>`, waarbij `<tenant domain>` is een van de domeinen gecontroleerd in de huurder of de eerste `<...>.onmicrosoft.com` domein. Dit type account is een account voor werk of school.

In een huurder Azure AD B2C de meeste apps wilt dat de gebruiker zich aanmelden met een willekeurig e-mailadres (bijvoorbeeld joe@comcast.net, bob@gmail.com, sarah@contoso.com, of jim@live.com). Dit type account is een lokale account. Vandaag, ondersteuning wij ook voor willekeurige gebruikersnamen (gewoon tekenreeksen) als lokale accounts (bijvoorbeeld Jan, bob, sarah of jim). U kunt een van deze twee lokale account in de Azure AD B2C-service.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welke sociale identiteitsproviders ondersteund nu? Welke u wilt u in de toekomst te ondersteunen?

We ondersteunen momenteel Facebook, Google +, LinkedIn en Amazon. We zullen ondersteuning toevoegen voor andere populaire sociale identiteitsproviders op basis van de vraag van klanten.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kan ik bereiken meer om informatie te verzamelen over de consumenten van verschillende aanbieders van sociale identiteit configureren?

Nee, maar deze functie is op onze roadmap. De standaard scopes voor onze ondersteunde identiteitsproviders van sociale gebruikt worden:

- Facebook: e-mail
- Google +: e-mail
- Microsoft-account: openid e-profiel
- Amazon: profiel
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Zijn mijn toepassing uitgevoerd op Azure voor het werken met Azure AD B2C?

Nee, u kunt hosten op uw toepassing overal (in de cloud of op locatie). Nodig is om te communiceren met Azure AD B2C, is de mogelijkheid om te verzenden en ontvangen van HTTP-aanvragen op openbaar toegankelijke eindpunten.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ik heb meerdere Azure AD B2C huurders. Hoe kan ik deze beheren op de Azure Portal?

Elke huurder Azure AD B2C heeft eigen functies B2C blade van de Azure portal. Zie [Azure AD B2C: registreren van uw toepassing](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) voor meer informatie over hoe u kunt navigeren naar een bepaalde huurder B2C functies blade van de Azure portal. Schakelen tussen mappen op de Azure portal Azure AD B2C bewaart uw B2C onderdelen blade openen in de meeste browsers.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Hoe pas ik de controle op e-mails (de inhoud en de ' van: ' veld) verzonden door Azure AD B2C?

De [huisstijl bedrijf-functie](../active-directory/active-directory-add-company-branding.md) kunt u de inhoud van een verificatie-e-mailberichten aanpassen. Met name kunnen deze twee elementen van het e-mailbericht worden aangepast:

- **Vaandel Logo**: weergegeven in de rechterbenedenhoek.
- **Achtergrondkleur**: boven weergegeven.

    ![Schermafdruk van een aangepaste verificatie e-mailadres](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

De e-handtekening bevat de naam van de huurder B2C die u hebt opgegeven bij het maken van de pachter B2C. Kunt u de naam van de volgende instructies:

- Inloggen op de [portal van Azure klassieke](https://manage.windowsazure.com/) als abonnement beheerder.
- Ga naar de huurder B2C.
- Klik op het tabblad **configureren** .
- Het veld **naam** in de sectie **Eigenschappen van de map** wijzigen.
- Klik op **Opslaan** onder aan de pagina.

Er is momenteel geen manier om de ' van: ' op het e-mailbericht. Bent u geïnteresseerd in deze mogelijkheid en volledig aanpassen van de hoofdtekst van de e-mail verificatie, stem voor de functie op [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Hoe kan ik migreren mijn bestaande gebruikersnamen, wachtwoorden en profielen van mijn database voor Azure AD B2C?

De Azure AD Graph API kunt u uw migratieprogramma schrijven. Zie het [voorbeeld van Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) voor meer informatie. We bieden verschillende opties voor de migratie en hulpprogramma's voor out-of-the-box in de toekomst.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welk beleid wordt gebruikt voor lokale accounts in Azure AD B2C?

Het wachtwoordbeleid Azure AD B2C voor lokale accounts is gebaseerd op het beleid voor Azure advertentie. Azure AD B2C de aanmelding, aanmelden of aanmelden en het wachtwoord beleid gebruikt de kracht van 'sterk' wachtwoord opnieuw en wachtwoorden niet verloopt. Lees de [Azure AD wachtwoordbeleid](https://msdn.microsoft.com/library/azure/jj943764.aspx) voor meer informatie.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kan ik migreren consument identiteiten die zijn opgeslagen op mijn Active Directory op ruimten naar Azure AD B2C Azure AD Connect gebruiken?

Nee, Azure AD verbinden is niet ontworpen voor gebruik met Azure AD B2C. We bieden verschillende opties voor de migratie en hulpprogramma's voor out-of-the-box in de toekomst.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Werkt Azure AD B2C met systemen zoals Microsoft Dynamics CRM?

Op dit moment niet. Integratie van deze systemen is op onze roadmap.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Is Azure AD B2C werken met SharePoint op-premises 2016 of eerder?

Op dit moment niet. Azure AD B2C heeft geen ondersteuning voor het SAML 1.1 tokens die portals en e-commerce-toepassingen die zijn gebaseerd op SharePoint op locatie nodig. Let op Azure AD B2C is niet bedoeld voor het SharePoint externe partner delen scenario; Zie [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) in plaats daarvan.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Moet ik gebruiken Azure AD B2C of B2B externe identiteiten beheren?

Lees dit artikel over [externe identiteiten](../active-directory/active-directory-b2b-compare-external-identities.md) voor meer informatie over het toepassen van de juiste functies voor uw externe identiteit-scenario's.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welke rapportering en controle functies Azure AD B2C biedt? Zijn ze hetzelfde als in Azure AD Premium?

Nee, Azure AD B2C ondersteunt niet dezelfde reeks rapporten als Azure AD Premium. Azure AD B2C brengt eenvoudige rapportage en API's snel te controleren.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Kan ik de gebruikersinterface van de pagina's die worden bediend door Azure AD B2C localiseren? Welke talen worden ondersteund?

Op dit moment Azure AD B2C geoptimaliseerd voor Engels alleen. Zijn we van plan zo spoedig mogelijk te lokaliseren functies implementeert.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kan ik mijn eigen URL's op mijn aanmeldings- en -in pagina's die worden verwerkt in Azure AD B2C gebruiken? Zo kan ik de URL van login.microsoftonline.com naar login.contoso.com wijzigen?

Op dit moment niet. Deze functie is op onze roadmap. Rekening mee dat voor het controleren van uw domein op het tabblad **domeinen** van de huurder op de klassieke Azure portal dit niet doet dus.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Hoe verwijder ik mijn huurder Azure AD B2C

Ga als volgt te werk als u wilt verwijderen van uw huurder Azure AD B2C:

- Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
- Ga naar de blades **toepassingen**, **identiteit, providers** en **alle beleidsregels** en verwijder alle vermeldingen in elk van hen.
- Nu aanmelden bij de [Azure klassieke portal](https://manage.windowsazure.com/) als abonnement beheerder. (Dit is het hetzelfde werk of school account of het Microsoft-account waarmee u zich aanmeldt voor Azure).
- Ga naar de Active Directory-extensie aan de linkerkant en klik op de huurder B2C.
- Klik op het tabblad **gebruikers** .
- Selecteer elke gebruiker in (de gebruiker u bent momenteel ingelogd als, d.w.z. het abonnement beheerder uitsluiten) inschakelen. Klik op **verwijderen** onder aan de pagina en klik op **Ja** wanneer u wordt gevraagd.
- Klik op het tabblad **toepassingen** .
- **Mijn bedrijf eigenaar is van toepassingen** selecteren in de vervolgkeuzelijst **weergeven** en klik op het vinkje.
- Hier ziet u een toepassing **b2c-extensies-app** die hieronder worden genoemd. Klik op **verwijderen** onder aan de pagina en klik op **Ja** wanneer u wordt gevraagd.
- Ga naar de Active Directory-extensie opnieuw en selecteer de huurder B2C.
- Klik op **verwijderen** onder aan de pagina. Volg de instructies op het scherm om het proces te voltooien.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Krijg ik Azure AD B2C als onderdeel van Enterprise Mobility Suite?

Nee, Azure AD B2C is een pay-as-you-go Azure service en is geen onderdeel van Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Hoe meld ik problemen met Azure AD B2C

Zie [ondersteuning aanvragen voor Azure Active Directory B2C-bestand](active-directory-b2c-support.md).

## <a name="more-information"></a>Meer informatie

U ook kunt bekijken van de huidige [beperkingen van de service, beperkingen en beperkingen](active-directory-b2c-limitations.md).

<properties
    pageTitle="Eenmalige aanmelding beheer voor enterprise apps in de voorvertoning Azure Active Directory | Microsoft Azure"
    description="Informatie over het beheren van eenmalige op voor enterprise apps met Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Voorbeeld: Beheren van eenmalige aanmelding voor enterprise toepassingen in de nieuwe portal Azure

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure klassieke portal](active-directory-sso-integrate-saas-apps.md)

In dit artikel wordt beschreven hoe de [Azure portal](https://portal.azure.com) gebruiken voor het beheren van instellingen voor eenmalige aanmelding voor toepassingen, met name de distributielijsten die zijn toegevoegd in de [galerie met Azure Active Directory (AD Azure) toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). De Azure AD management ervaring voor eenmalige aanmelding is momenteel in openbare preview en in dit artikel beschrijft de nieuwe functies zoals een paar tijdelijke beperkingen die alleen tijdens de proefperiode in plaats zijn. [Wat is in het voorbeeld?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Zoeken naar uw toepassingen in de nieuwe portal

Vanaf September 2016, kunnen alle toepassingen die zijn geconfigureerd voor één teken op in een map, door een beheerder van een directory met behulp van de [galerie met Azure Active Directory-toepassing](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) binnen de [Azure klassieke portal](https://manage.windowsazure.com), nu worden bekeken en beheerd in de portal voor Azure.

Deze toepassingen vindt u in de sectie **Zakelijke toepassingen** van de Azure portal, een koppeling naar die kan worden gevonden in de lijst **Meer Services** in de [portal](https://portal.azure.com). Enterprise apps zijn apps die zijn geïmplementeerd en worden gebruikt door gebruikers binnen uw organisatie.

![Bedrijfstoepassingen blade][1]

Selecteer **alle toepassingen** een lijst weergeven van alle toepassingen die zijn geconfigureerd, met inbegrip van toepassingen die vanuit de galerie is toegevoegd. Selecteren van een app laadt het blad van de resource voor die toepassing, waar rapporten kunnen worden weergegeven voor die toepassing en verschillende instellingen kan worden beheerd.

Selecteer **eenmalige aanmelding**voor het beheren van instellingen voor eenmalige aanmelding.

![Application resource blade][2]


##<a name="single-sign-on-modes"></a>Single sign-on-modi

De **Single sign-on** -blade begint met een menu **modus** , kunt u de modus voor één aanmelding moet worden geconfigureerd. De volgende opties zijn beschikbaar:

* **Teken op op basis van SAML** - deze optie is beschikbaar als de toepassing ondersteunt volledige federatieve eenmalige aanmelding met Azure Active Directory met behulp van het protocol van SAML 2.0.

* **Teken op wachtwoorden gebaseerde** - deze optie is beschikbaar als Azure AD wachtwoord formulier invullen voor deze toepassing ondersteunt.

* **Gekoppelde aanmelden** - voorheen bekend als "bestaande single sign-on', deze optie beheerders kunnen plaats een koppeling naar deze toepassing in van de gebruiker paneel Azure AD Access of Office 365-toepassing starten.

Zie voor meer informatie over deze modi [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>Op basis van SAML-teken op

De optie **aanmelden op SAML gebaseerde** bevat een bladeserver die is verdeeld in vier secties:

###<a name="domains-and-urls"></a>Domeinen en URL 's
Dit is waar alle details over het domein en de URL's van de toepassing worden toegevoegd aan de map Azure AD. Overwegende dat alle optionele invoer kunnen worden weergegeven door het selectievakje **weergeven geavanceerde instellingen voor URL** alle ingangen die nodig zijn om één aanmelding werk app rechtstreeks op het scherm weergegeven. De volledige lijst van ondersteunde "inputs" bevat:

* **Teken op URL** – waar de gebruiker om aan te melden voor deze toepassing gaat. Als de toepassing is geconfigureerd voor het uitvoeren van service provider geïnitieerde eenmalige op, vervolgens wanneer een gebruiker naar deze URL navigeert, heeft de serviceprovider de nodige omleiding naar Azure AD worden geverifieerd en de gebruiker aanmelden. Als dit veld is ingevuld, vervolgens Azure AD gebruikt deze URL om de toepassing van Office 365 en Azure AD toegang te starten. Als in dit veld wordt weggelaten, wordt in plaats daarvan identiteitsprovider Azure AD voert-initiated teken op als de toepassing is gestart vanuit Office 365, Azure AD Access het deelvenster of de Azure advertentie enkel URL aanmelding.

* **Id** - deze URI moet worden geïdentificeerd via de toepassing voor welke één aanmelden is geconfigureerd. Dit is de waarde die Azure AD teruggestuurd naar de toepassing als de parameter voor de doelgroep van het SAML-token wordt en de toepassing naar verwachting zal valideren. Deze waarde wordt ook weergegeven als de entiteit-ID in de SAML-metagegevens die door de toepassing.

* **Antwoord URL** - de URL van het antwoord is waar de toepassing wordt verwacht te ontvangen van het SAML-token. Dit is ook de URL bevestiging consument Service (ACS) genoemd. Nadat u deze hebt ingevoerd, klikt u op volgende om door te gaan naar het volgende scherm. Dit scherm bevat informatie over wat er worden geconfigureerd op de kant van de toepassing moet om het te accepteren van een SAML-token uit Azure Active Directory.

* **Relay staat** - de status van de relay is een optionele parameter die kan helpen bij het geven van de toepassing waar de gebruiker omleiden nadat de verificatie is voltooid. De waarde is meestal een geldige URL op van de toepassing, maar sommige toepassingen anders dit veld gebruikt (Zie de app van eenmalige documentatie voor meer informatie). De mogelijkheid om de status van de relay is een nieuwe functie die uniek is voor de nieuwe portal Azure.

###<a name="user-attributes"></a>Gebruikerskenmerken
Dit is waar beheerders kunnen bekijken en bewerken van kenmerken die in het SAML-token die Azure AD aan de toepassing uitgeeft worden verzonden wanneer gebruikers zich aanmelden.

Voor de eerste preview-versie is het alleen bewerkbaar kenmerk ondersteund het kenmerk **Id van de gebruiker** . De waarde van dit kenmerk is het veld in Azure AD met een unieke identificatie van elke gebruiker in de toepassing. Bijvoorbeeld als de toepassing is geïmplementeerd met behulp van het "e-mailadres" Als de gebruikersnaam en het unieke id, vervolgens de waarde zou worden ingesteld op het veld 'user.mail' in Azure AD.

Het bewerken van aanvullende kenmerken worden ondersteund in het volgende voorbeeld.

###<a name="saml-signing-certificate"></a>Handtekeningcertificaat SAML
Dit gedeelte toont de details van het certificaat dat Azure AD wordt gebruikt voor het ondertekenen van de SAML-tokens die worden uitgegeven aan de toepassing telkens wanneer de gebruiker wordt geverifieerd. Kan de eigenschappen van het huidige certificaat moet worden gecontroleerd, met inbegrip van de vervaldatum.

De mogelijkheid om het certificaat rollover en aanvullende verklaring opties worden ondersteund in een latere preview-versie beheren. Opmerking: het volledige beheer van certificaten kan nog steeds worden uitgevoerd in de [Azure klassieke portal](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Toepassingsconfiguratie
In het laatste gedeelte bevat documentatie en/of besturingselementen vereist voor het configureren van de toepassing zelf Azure Active Directory als een id-provider gebruiken.

Het menu van het vervolgmenu **Toepassing configureren** biedt nieuwe beknopte, ingesloten instructies voor het configureren van de toepassing. Dit is een andere nieuwe functie die uniek zijn voor de nieuwe portal Azure.

> [AZURE.NOTE] Zie voor een volledig voorbeeld van ingesloten documentatie bij de toepassing Salesforce.com. Documentatie voor extra apps wordt voortdurend toegevoegd tijdens de voorvertoning.

![Ingesloten documenten][3]

##<a name="password-based-sign-on"></a>Teken op basis van wachtwoorden op
Als de toepassing wordt ondersteund, configureert de wachtwoord gebaseerde SSO-modus selecteren en direct **Opslaan** selecteert u eenmalige aanmelding op basis van wachtwoorden. Zie voor meer informatie over de implementatie van eenmalige aanmelding op basis van wachtwoorden, [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Teken op basis van wachtwoorden op][4]


##<a name="linked-sign-on"></a>Gekoppelde teken op
Als de toepassing wordt ondersteund, kunt de gekoppelde SSO-modus u de URL die u wilt het deelvenster Azure AD Access of Office 365 om te leiden wanneer gebruikers op deze app invoeren. Zie voor meer informatie over gekoppelde SSO (voorheen bekend als 'bestaande SSO'), [hoe eenmalige aanmelding met Azure Active Directory werk](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Gekoppelde aanmelding][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG

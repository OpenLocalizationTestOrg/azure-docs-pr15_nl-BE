<properties
    pageTitle="Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport | Microsoft Azure"
    description="Overzicht van Microsoft Passport en extra informatie over het implementeren van Microsoft Passport."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Verificatie van identiteiten zonder wachtwoorden via Microsoft Passport

De huidige methoden van verificatie met wachtwoorden alleen zijn niet voldoende om gebruikers te beschermen. Gebruikers gebruiken en wachtwoorden vergeten. Wachtwoorden zijn breachable, phishable, gevoelig voor scheuren en te raden. Ze krijgen ook moeilijk te onthouden en gevoelig voor aanvallen zoals '[geeft u de hash](https://technet.microsoft.com/dn785092.aspx)'.

## <a name="about-microsoft-passport"></a>Info over Microsoft Passport
Microsoft Passport is een persoonlijke/openbare sleutel of op certificaten gebaseerde verificatie voor organisaties en consumenten die verder dan wachtwoorden gaat. Deze vorm van verificatie is afhankelijk van sleutelpaar referenties die wachtwoorden kunnen vervangen en zijn bestand tegen inbreuken, diefstal en phishing.

 Microsoft Passport kunt een gebruiker worden geverifieerd bij een Microsoft-account, een Windows Server Active Directory-account, een account van Microsoft Azure Active Directory (AD Azure) of een niet-Microsoft-service die snel identiteit Online (FIDO) verificatie ondersteunt. Na een controle van de eerste twee stappen tijdens het inschrijven van Microsoft Passport, Microsoft Passport is ingesteld op het apparaat van de gebruiker en de gebruiker een penbeweging Windows Hello of PIN-code kan worden ingesteld. De gebruiker geeft de beweging om hun identiteit te verifiëren. Vervolgens wordt Microsoft Passport gebruikt voor de verificatie van de gebruiker en waarmee ze toegang krijgen tot beveiligde bronnen en services.

De persoonlijke sleutel is beschikbaar gesteld uitsluitend via een "beweging van de gebruiker" zoals een PINCODE, biometrie of een extern apparaat zoals een smartcard, waarmee de gebruiker zich aanmelden bij het apparaat. Deze informatie is gekoppeld aan een certificaat of een combinatie van een asymmetrische sleutel. De persoonlijke sleutel is een hardware-gestaafd als het apparaat een chip Trusted Platform Module (TPM heeft). De persoonlijke sleutel blijft nooit het apparaat.

De openbare sleutel wordt geregistreerd met Azure Active Directory en Windows Server Active Directory (voor op gebouwen). ID-Providers (IDPs) de gebruiker valideren door de openbare sleutel van de gebruiker toe te wijzen aan de persoonlijke sleutel en informatie aanmelden via één keer wachtwoord (OTP), PhoneFactor of een andere melding mechanisme.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Waarom ondernemingen dient vast te stellen Microsoft Passport

Microsoft Passport is ingeschakeld, ondernemingen kunnen hun bronnen zelfs beter beveiligen door:

* Microsoft Passport instellen met een optie voor de voorkeurstaal van hardware. Dit betekent dat sleutels worden gegenereerd op TPM 1.2 of 2.0 TPM indien beschikbaar. Als de TPM niet beschikbaar is, zal de software de sleutel genereren.

* De complexiteit en de lengte van de PINCODE, definiëren en of het gebruik van Hello is ingeschakeld in uw organisatie.

* Configureren van Microsoft Passport voor smartcard-achtige scenario's met behulp van vertrouwen op basis van certificaten.

## <a name="how-microsoft-passport-works"></a>De werking van Microsoft Passport
1. Sleutels worden gegenereerd op de hardware door de TPM of software. Veel apparaten hebben een ingebouwde TPM-chip die de hardware wordt beveiligd door de integratie van cryptografische sleutels in apparaten. TPM 1.2 of 2.0 van de TPM genereert sleutels of certificaten die zijn gemaakt op basis van de gegenereerde sleutels.

2. De TPM verklaart deze hardware-gebonden toetsen.

3. Een penbeweging één ontgrendelen ontgrendelt het apparaat. Dit gebaar kunt toegang tot meerdere bronnen als het apparaat deel uitmaakt van een domein of Azure advertentie toegevoegd.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>De werking van de levenscyclus van Microsoft Passport

![Levenscyclus van Microsoft Passport](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Het bovenstaande diagram illustreert de persoonlijke/openbare sleutelpaar en de validatie van de identiteitsprovider. Elk van deze stappen wordt uitgelegd in detail hier:

1. De gebruiker hun identiteit bewijst door meerdere ingebouwde taalprogramma methoden (penbewegingen, fysieke smartcards, meerledige verificatie) en stuurt deze gegevens naar een Identity Provider (IDP) zoals Azure Active Directory of Active Directory ruimten.

2. Het apparaat vervolgens maakt u de sleutel, verklaart de sleutel duurt het openbare gedeelte van deze sleutel, worden deze gekoppeld met overzichten van station, ondertekent en stuurt dit naar de IDP de sleutel te registreren.

4. Zodra de IDP het openbare gedeelte van de sleutel registreert, vraagt de IDP het apparaat aan te melden met het persoonlijke gedeelte van de sleutel.

5. De IDP vervolgens valideert problemen en de verificatietoken waarmee de gebruiker en de toegang tot de beveiligde bronnen. IDPs kan cross-platform apps schrijven of browserondersteuning (via JavaScript/Webcrypto-API's) maken en gebruiken van referenties van Microsoft Passport voor gebruikers.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>De eisen voor Microsoft Passport
### <a name="at-the-enterprise-level"></a>Op het ondernemingsniveau van de

* De onderneming heeft een Azure-abonnement.

### <a name="at-the-user-level"></a>Op het gebruikersniveau van de

* De computer wordt uitgevoerd Windows 10 Professional of Enterprise.

Zie voor gedetailleerde installatie-instructies [Inschakelen van Microsoft Passport voor werk in de organisatie](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Als u meer informatie

* [Windows 10 voor de onderneming: apparaten gebruik voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud-mogelijkheden aan Windows 10-apparaten via Azure Active Directory deelnemen aan uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over gebruiksscenario's voor Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Domein behoren apparaten aansluiten op Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD Join instellen](active-directory-azureadjoin-setup.md)

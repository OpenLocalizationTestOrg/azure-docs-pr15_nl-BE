<properties
    pageTitle="Azure AD in Duitsland Microsoft Cloud verbinden"
    description="Azure AD verbinden integreert uw mappen op de ruimten met Azure Active Directory. Hiermee kunt u een gemeenschappelijke identiteit bieden voor Office 365, Azure en SaaS-toepassingen geïntegreerd met AD Azure."
    keywords="Inleiding op Azure AD Connect, Azure AD verbinding maken met, overzicht, wat is Azure AD Connect, active directory, Duitsland, Black Forest installeren"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD verbinding in Duitsland Microsoft Cloud - Public Preview

## <a name="introduction"></a>Inleiding
Azure AD Connect biedt synchronisatie tussen uw Active Directory op gebouwen en Azure Active Directory.
Op dit moment moeten veel van de scenario's in [Microsoft Cloud Duitsland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) gebeuren door de operator. Wanneer u Microsoft Cloud Duitsland, moet u zich bewust zijn van de volgende:


- De volgende URL's moeten worden geopend op een proxy-server voor de synchronisatie met succes wordt uitgevoerd:
    - *. microsoftonline.de
    - *. windows.net
    - + Certificaatintrekkingslijsten

- U aanmelden bij de Azure AD-map, moet u een account in het domein onmicrosoft.de.
- De volgende functies zijn niet beschikbaar:
    - Azure AD verbinding gezondheid
    - Automatische updates
    - Wachtwoord Write-back

## <a name="download"></a>Downloaden
Azure AD Connect kunt u downloaden van de bladeserver Azure AD verbinden binnen de portal.  Gebruik de onderstaande instructies te vinden van de bladeserver Azure AD verbinden.

### <a name="the-azure-ad-connect-blade"></a>De Azure AD verbinding Blade

Zodra u hebt aangemeld bij de portal Azure, het volgende doen:

1. Ga naar bladeren
2.  Selecteer Azure Active Directory
3.  Selecteer Azure AD verbinden

Zie de volgende:

![Azure AD verbinding Blade](media\active-directory-aadconnect-germany\germany1.png)

 
De volgende tabel beschrijft de functies die in het blad.


Titel|Beschrijving|
----- | ----- |
SYNC-STATUS|Laten we weten u of de synchronisatie is ingeschakeld of uitgeschakeld.|
LAATSTE SYNCHRONISATIE|De laatste keer dat een geslaagde synchronisatie is voltooid.|
FEDERATIEVE DOMEINEN|Bevat het nummer van de federatieve domeinen die momenteel zijn geconfigureerd.|


## <a name="installation"></a>Installatie
Om te verbinden met Azure AD installeert, kunt u de documentatie [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Geavanceerde functies en aanvullende informatie
Beginnen met het [integreren van identiteiten in ruimten met Azure Active Directory](active-directory-aadconnect.md)voor meer informatie en richtlijnen voor aangepaste instellingen of geavanceerde configuraties.  Deze pagina biedt informatie en koppelingen naar aanvullende richtlijnen.

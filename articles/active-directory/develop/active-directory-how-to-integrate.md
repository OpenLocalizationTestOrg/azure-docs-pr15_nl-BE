<properties
   pageTitle="Integreren met Azure Active Directory | Microsoft Azure"
   description="Een handleiding voor de voordelen van en bronnen voor integratie met Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integratie met Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory biedt organisaties met een enterprise-grade Identiteitsbeheer voor cloud-toepassingen.  Biedt uw gebruikers gestroomlijnde ervaring-in Azure AD-integratie en helpt u uw toepassing in overeenstemming zijn met het beleid voor IT.

## <a name="how-to-integrate"></a>Het integreren van

Er zijn verschillende manieren voor uw toepassing integreren met AD Azure.  Profiteer van veel of weinig van deze scenario's is voor uw toepassing.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Azure AD om het aanmelden bij uw toepassing te ondersteunen

**Inloggen wrijving reduceren en besparen op ondersteuningskosten.** Uw-gebruikers geen via Azure AD aan te melden uw toepassing een meer naam en wachtwoord te onthouden.  Als ontwikkelaar hebt u minder een wachtwoord voor het opslaan en beschermen.  Niet te vergeten wachtwoord opnieuw verwerken met mogelijk een aanzienlijke besparing alleen.  Inloggen stimulans Azure AD voor een aantal van's werelds meest populaire cloud toepassingen, waaronder Office 365 en Microsoft Azure.  Met honderden miljoenen gebruikers uit miljoenen organisaties, kansen zijn uw gebruikersnaam is al aangemeld bij Azure AD.  Meer informatie over [toe te voegen ondersteuning voor Azure AD inloggen](../active-directory-authentication-scenarios.md).

**Sign up vereenvoudigen voor uw toepassing.**  Tijdens het aanmelden van uw toepassing kunt Azure AD essentiële informatie over een gebruiker verzenden, zodat u kunt uw aanmelding formulier alvast in te vullen of deze volledig te elimineren.  Gebruikers kunnen aanmelden voor uw toepassing met hun Azure AD-account via een bekende toestemming ervaring vergelijkbaar met die gevonden in de sociale media en mobiele toepassingen.  Elke gebruiker kunt aanmelden en aanmelden bij een toepassing die is geïntegreerd met AD Azure zonder IT-middelen.  Meer informatie over het [ondertekenen van uw toepassing voor Azure AD-Account inloggen](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Zoeken naar gebruikers, gebruikersaanvragen beheren en toegang tot de toepassing

**Zoeken naar gebruikers in de directory.**  De Graph API gebruiken om u te helpen gebruikers zoeken en voor andere mensen in hun organisatie wanneer andere gebruikers uitnodigen of verlenen van toegang, zodat ze niet te typen van e-mail adressen.  Gebruikers kunnen bladeren met behulp van een bekend adresboek stijlinterface, inclusief de details weergeven van de organisatiehiërarchie.  Meer informatie over de [API van de grafiek](../active-directory-graph-api.md).

**Active Directory-groepen en de klant is al het beheren van distributielijsten opnieuw gebruiken.**  Azure AD bevat de groepen die uw klant al wordt gebruikt voor e-verdeling en toegangsbeheer.  Met de API van de grafiek, deze groepen zodat uw klanten maken en beheren van een afzonderlijke set groepen in uw toepassing niet opnieuw gebruiken.  Gegevens kan ook worden verzonden naar de toepassing in tokens inloggen.  Meer informatie over de [API van de grafiek](../active-directory-graph-api.md).

**Azure AD om te bepalen wie toegang tot uw toepassing heeft gebruiken.**  Beheerders en eigenaars van de toepassing in Azure AD kunnen toegang geven tot toepassingen voor specifieke gebruikers en groepen.  Graph API gebruiken, kunt u deze lijst lezen en inrichten en verwijderen van gegevens van resources te beheren en openen vanuit uw toepassing.

**Gebruik AD Azure voor rollen gebaseerd toegangsbeheer.**  Beheerders en eigenaars van de toepassing kunnen gebruikers en groepen toewijzen aan rollen die u definieert wanneer u de toepassing in Azure AD registreert.  Functie-informatie wordt verzonden naar uw toepassing in tokens inloggen en kan ook worden gelezen met behulp van de API van de grafiek.  Meer informatie over het [gebruik van Azure AD voor autorisatie](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Toegang krijgen tot het profiel van de gebruiker, agenda, E-mail, contactpersonen, bestanden en meer

**Azure AD is de server van de vergunning voor Office 365 en overige zakelijke diensten van Microsoft.**  Als u ondersteuning voor Azure AD voor aanmelden bij uw toepassing of uw huidige gebruikersaccounts koppelen aan gebruikersaccounts Azure AD met OAuth 2.0-ondersteuning, kunt u lees- en schrijftoegang tot het profiel van een gebruiker, agenda, e-mail, contactpersonen, bestanden en andere informatie aanvragen.  U kunt probleemloos gebeurtenissen aan de agenda van de gebruiker, schrijven en lezen of schrijven van bestanden op de OneDrive.  Meer informatie over het [verkrijgen van toegang tot de Office 365-API](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Bevordering van de toepassing in de Azure en Office 365 marktplaatsen

**Bevordering van de toepassing naar de miljoenen van organisaties die al met Azure AD.**  Gebruikers die zoeken en deze marktplaatsen al gebruikmaakt van een of meer cloud-services, waardoor ze gekwalificeerd cloud service-klanten.  Meer informatie over de bevordering van de toepassing in [De markt Azure](https://azure.microsoft.com/marketplace/partner-program/).

**Wanneer gebruikers zich voor uw toepassing aanmelden, verschijnt het in hun deelvenster Azure AD-toegang en Office 365-app starten.**  Gebruikers kunnen snel en gemakkelijk terugkeren naar de toepassing later kan betrokkenheid bij de gebruiker te verbeteren.  Meer informatie over de [Azure AD toegang hebt tot het Configuratiescherm](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Beveiligde communicatie-apparaat te onderhouden en services

**Azure AD gebruiken voor het identiteitsbeheer van de services en apparaten vermindert de code hoeft te schrijven en kunnen IT-toegang beheren.**  Services en apparaten tokens uit Azure Active Directory gebruikt OAuth krijgen en deze tokens gebruiken voor toegang tot web-API's.  Met behulp van Azure AD kunt u voorkomen dat complexe verificatiecode schrijft.  Omdat de identiteit van de apparaten en services worden opgeslagen in AD Azure, IT-sleutels en intrekking op één plaats hoeft hiervoor afzonderlijk in uw toepassing kunt beheren.

## <a name="benefits-of-integration"></a>Voordelen van integratie

Integratie met AD Azure biedt voordelen die niet vereisen dat u extra code schrijven.

### <a name="integration-with-enterprise-identity-management"></a>Integratie met Enterprise Identity Management

**Help uw toepassing voldoen aan IT-beleid.**  Organisaties hun onderneming identity management systemen integreren met AD Azure, zodat wanneer een persoon een organisatie verlaat, ze automatisch geen toegang meer tot uw toepassing zonder IT die behoefte hebben aan extra maatregelen te nemen.  IT kunt beheren die toegang tot uw toepassing en vaststellen welk toegangsbeleid vereist - voor voorbeeld meerledige verificatie - waarom u moet code schrijven om te voldoen aan de complexe bedrijfsbeleid verminderen zijn.  AD Azure biedt beheerders een gedetailleerde controlelogboek van die aangemeld bij uw toepassing zodanig IT gebruik kunt bijhouden.

**Azure AD breidt Active Directory naar de cloud, zodat uw toepassing met AD integreren kunt.**  Veel organisaties over de hele wereld Active Directory gebruiken als hun belangrijkste aanmelden en identiteit management system en moeten hun toepassingen te werken met Active Directory.  Uw app integreren met Azure Active Directory is geïntegreerd met Active Directory.

### <a name="advanced-security-features"></a>Geavanceerde beveiligingsfuncties

**Meerledige verificatie.**  AD Azure biedt native meerledige verificatie.  IT-beheerders kunnen meerledige verificatie toegang te krijgen tot uw toepassing, moet zodanig dat er geen ondersteuning voor deze code zelf.  Meer informatie over [Meerledige verificatie](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Afwijkende inloggen detectie.**  Azure AD verwerkt meer dan een miljard aanmeldingen per dag, tijdens het gebruik van machine learning algoritmen voor verdachte activiteiten te detecteren en de kennisgeving van mogelijke problemen met IT-beheerders.  Uw toepassing krijgt door de ondersteuning van Azure AD sign-in het voordeel van deze bescherming. Meer informatie over [access-rapport weergeven van Azure Active Directory](../active-directory-view-access-usage-reports.md).

**Voorwaardelijke toegang.**  Naast een meerledige verificatie, kunnen beheerders opgeven dat bepaalde voorwaarden worden voldaan voordat gebruikers kunnen aanmelden bij uw toepassing.  Voorwaarden die u kunnen instellen zijn het IP-adresbereik van clientapparaten, lidmaatschap van de opgegeven groepen en de status van het apparaat wordt gebruikt om toegang te krijgen.  Meer informatie over [voorwaardelijke toegang Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Eenvoudige ontwikkeling

**Industriestandaardprotocollen.**  Microsoft hecht aan de ondersteuning van industriestandaarden.  Azure AD ondersteunt de verificatieprotocollen SAML 2.0, OpenID verbinding 1.0, 2.0 OAuth en WS-Federation 1.2.  De Graph API is OData 4.0 compatibel.  Als de toepassing al de protocollen van SAML 2.0 of OpenID verbinding 1.0 voor federatieve inloggen ondersteunt, kan toevoegen ondersteuning voor Azure advertentie eenvoudig zijn.  Meer informatie over [verificatieprotocollen Azure AD ondersteund](../active-directory-authentication-protocols.md).

**Open source-bibliotheken.**  Microsoft biedt een volledig ondersteunde open source bibliotheken voor populaire talen en platformen ontwikkelen versnellen.  De broncode is gelicentieerd onder een Apache 2.0 en u bent vrij om zich splitsen en bij te dragen aan de projecten.  Meer informatie over [AD Azure verificatie bibliotheken](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Wereldwijde aanwezigheid en beschikbaarheid

**Azure AD wordt in datacenters over de hele wereld wordt geïmplementeerd en beheerd en bewaakt de klok rond.**  Azure AD is het identity management system voor Microsoft Azure en Office 365 en wordt geïmplementeerd in 28 datacenters over de hele wereld.  Active Directory-gegevens is worden gerepliceerd naar ten minste drie datacenters gegarandeerd.  Netwerktaakverdeling voor algemene gebruikers toegang tot de dichtstbijzijnde kopie van Azure AD met hun gegevens waarborgen en aanvragen naar andere datacenters automatisch opnieuw verzenden als er een probleem is aangetroffen.

## <a name="next-steps"></a>Volgende stappen

Het [schrijven van code aan de slag](../active-directory-developers-guide.md#getting-started).

[Gebruikers In met behulp van Azure AD Sign](../active-directory-authentication-scenarios.md)

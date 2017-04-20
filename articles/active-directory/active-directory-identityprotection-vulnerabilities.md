<properties
    pageTitle="Beveiligingsproblemen ontdekt door Azure Active Directory identiteitsbeveiliging | Microsoft Azure"
    description="Overzicht van de beveiligingslekken die zijn gedetecteerd door identiteitsbeveiliging van Azure Active Directory."
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Beveiligingsproblemen ontdekt door Azure Active Directory identiteit bescherming 

Beveiligingslekken zijn zwakke punten in uw omgeving die kan worden misbruikt door een aanvaller. Het is raadzaam deze beveiligingslekken ter verbetering van de houding van de beveiliging van uw organisatie en dat misbruik van deze aanvallers.
<br><br>
![zwakke plekken](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

De volgende secties vindt u een overzicht van de beveiligingslekken die zijn gemeld door de bescherming van de identiteit.

## <a name="multi-factor-authentication-registration-not-configured"></a>Meerledige verificatie inschrijving niet geconfigureerd 

Dit beveiligingslek, kunt u de implementatie van Azure meerledige verificatie in uw organisatie beheren. 

Azure meerledige verificatie biedt een tweede laag van beveiliging voor verificatie van de gebruiker. Het helpt bij toegang tot gegevens en toepassingen veilig bij het voldoen aan eisen van de gebruiker voor een eenvoudig aanmelden proces. Sterke verificatie via een reeks opties voor eenvoudige verificatie levert, telefoongesprek, tekstbericht of mobiele app kennisgevings- of code en 3de partij EDE tokens.

We raden u aan Azure meerledige verificatie voor aanmeldingen van gebruikers. Meerledige verificatie speelt een sleutelrol in het beleid van voorwaardelijke toegang op basis van risico's beschikbaar zijn via de bescherming van de identiteit.

Zie voor meer informatie [Wat is Azure meerledige verificatie?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Niet-beheerde cloud-apps

Dit beveiligingslek helpt u bij het identificeren van onbeheerde cloud-toepassingen in uw organisatie.
 
In moderne ondernemingen zijn IT-afdelingen vaak niet bewust van alle cloud-toepassingen die van gebruikers in hun organisatie gebruikmaken kunnen werken. Het is gemakkelijk te zien waarom beheerders zou vragen hebt over ongeoorloofde toegang tot bedrijfsgegevens, mogelijke gegevens lekken en andere beveiligingsrisico's. 

Wij raden u aan dat uw organisatie implementeert Cloud App Discovery te ontdekken zonder begeleiding cloud-toepassingen en voor het beheer van deze toepassingen met Azure Active Directory.

Zie voor meer informatie, [Zoeken naar niet-beheerde cloud-toepassingen met Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Beveiligingswaarschuwingen van beschermde Identity Management

Dit beveiligingslek helpt u bij het opsporen en oplossen van waarschuwingen over beschermde identiteiten in uw organisatie.  

Als u wilt dat gebruikers kunnen uitvoeren van beschermde verrichtingen, organisaties moeten gebruikers tijdelijk of permanent bevoorrechte toegang verlenen in Azure AD, Azure of Office 365 resources of andere SaaS-apps. Elk van deze bevoorrechte gebruikers vergroot de kwetsbaarheid van uw organisatie. Dit beveiligingslek helpt u bij het identificeren van gebruikers met onnodige bevoorrechte toegang en passende maatregelen te verminderen of te elimineren van het risico dat met zich meedraagt. 

Wij raden aan dat de organisatie worden met Azure AD beschermde Identity Management, beheren en monitor identiteiten en hun toegang tot bronnen in Azure AD, alsmede andere Microsoft online services, zoals Office 365 of Microsoft Intune bevoorrecht.

Zie voor meer details [Azure AD beschermde Identity Management](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Zie ook

 - [Identiteitsbeveiliging Azure Active Directory](active-directory-identityprotection.md)

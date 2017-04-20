<properties
   pageTitle="Mogelijkheden voor het beheren van externe identiteiten met Azure Active Directory vergelijken | Microsoft Azure"
   description="Samenwerking Azure Active Directory B2B, B2C en meerdere huurder App vergelijkt voor de ondersteuning van verificatie en machtiging voor externe identiteiten"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Mogelijkheden voor het beheren van externe identiteiten met Azure Active Directory vergelijken

Naast het beheren van mobiele werknemers toegang tot apps SaaS Azure Active Directory (AD Azure) kan uw organisatie helpen om bronnen te delen met zakelijke partners en toepassingen voor bedrijven en consumenten leveren.

## <a name="developing-applications-for-businesses"></a>Ontwikkeling van toepassingen voor bedrijven

Biedt u een service of toepassing, zoals een service payroll bedrijven? AD Azure biedt de identiteit platform waarmee u toepassingen maken die naadloos integreren met miljoenen organisaties die Azure AD al geconfigureerd als onderdeel van de implementatie van Office 365 of andere enterprise services.

**Voorbeeld:** Een distributeur farmaceutische biedt medische artikelen en informatiesystemen voor de gezondheidszorg. Ze moesten veld een analytics toepassing medische praktijken en gewenste klanten hun eigen identiteiten beheren. Dit bedrijf hebt gekozen Azure AD als het platform identiteit voor de toepassing van de management praktijk Azure AD identiteiten hun klanten bieden teken omhoog wanneer dat nodig is. Zie [handleiding voor ontwikkelaars van Azure Active Directory](active-directory-developers-guide.md)voor meer informatie.

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Zakenpartner, toegang tot uw bedrijfsresources inschakelen

Hebt u zakelijke partners of anderen buiten uw bedrijf die toegang moeten krijgen tot de ondernemingsresources, zoals een SharePoint-site of uw ERP-systeem? Azure AD kan beheerders toe te staan van externe gebruikers (die mogelijk of bestaat niet in Azure AD) eenmalige toegang tot bedrijfstoepassingen. Dit verbetert de beveiliging als gebruikers toegang verliezen wanneer ze de partnerorganisatie verlaten terwijl u-beleid binnen uw organisatie bepalen. Dit vereenvoudigt ook het beheer als u niet nodig hebt voor het beheren van een externe partner directory of per Federatie partnerrelaties.

**Voorbeeld:** Een imaging-bedrijf biedt detailhandelaren photo imaging services en het grootste retail-netwerk van kiosken afdrukken werkt. Azure AD om duizenden gebruikers bij hun retail business partners hun eigen referenties gebruiken om te downloaden van het meest recente marketingmateriaal van partner en opnieuw ordenen van foto verwerking van voorraden van het bedrijf van leverancier extranet gekozen. Zie voor meer informatie [Azure AD B2B samenwerking](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Ontwikkeling van toepassingen voor de consument

Moet u voor het publiceren van online toepassingen, zoals een retail store front, miljoenen consumenten veilig en voordelig? AD Azure biedt een platform waardoor sociale en gebruikersnaam en wachtwoord aanmelden, huisstijl self-service aanmelden en wachtwoord zelf opnieuw ingesteld voor de consument van uw toepassing. Dit verhoogt gebruiksgemak voor uw gebruikers bij het verminderen van de belasting op de ontwikkelaars.

**Voorbeeld:** De \#1 Sport franchise ter wereld die rechtstreeks koppelen aan de 450 miljoen ventilatoren nodig zijn. U doet dit door ze een mobiele app met Azure AD voor gebruiker verificatie- en opslag gebouwd. Ventilatoren ophalen vereenvoudigde registratie en aanmelden door het gebruik van sociale accounts, zoals Facebook of ze traditionele gebruikersnaam/wachtwoord kunnen gebruiken voor een naadloze oplossing voor iOS, Android en Windows telefoons. Voortbouwend op de bestaande AD Azure platform aanzienlijk minder aangepaste code waardoor de franchise aangepaste huisstijl en verlichten bezorgdheid over beveiliging, inbreuken op de gegevens en schaalbaarheid. Zie voor meer informatie [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Vergelijking van de mogelijkheden van Azure AD

De scenario's die eerder in dit artikel wordt besproken is gericht door de mogelijkheden in Azure AD. Deze tabel moet helpen verduidelijken welke mogelijkheden zijn het meest relevant:

| **Houd rekening met dit product...**       | [Azure AD meerdere huurder SaaS-app](active-directory-developers-guide.md)    | [Azure AD B2B-samenwerking](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Als ik moet opgeven...** | een service voor bedrijven | partner toegang tot mijn apps  | een service voor de consument |
| **En ik ben ongeveer...**  | Pharma distributeur      | Imaging-bedrijf            | Sport-franchise       |
| **Implementeren van een app voor...**  | Praktijk management     | Leverancier extranet          | Voetbalfans            |
| **Doelitems van het type...**        | De kantoren van de arts        | Goedgekeurde zakelijke partners | Iedereen met een e-mailadres      |
| **Toegankelijk wanneer...**      | Klant admin toestemmingen | Mijn admin nodigt           | De consument zich aanmeldt      |

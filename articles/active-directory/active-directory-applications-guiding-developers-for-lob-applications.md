<properties
    pageTitle="Azure AD en toepassingen: Wegwijzer ontwikkelaars | Microsoft Azure"
    description="Geschreven voor IT-professionals, bevat in dit artikel richtlijnen voor Azure toepassingen integreren met Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD en toepassingen: line-of-business-toepassingen ontwikkelen

Deze handleiding bevat een overzicht van de ontwikkeling van toepassingen van line of business (LoB) voor Azure Active Directory (AD). De doelgroep is de globale beheerders Active Directory/Office 365.

## <a name="overview"></a>Overzicht

Bouwen van toepassingen die zijn geïntegreerd met AD Azure geeft gebruikers in uw organisatie single sign-on met Office 365. Dat de toepassing in Azure AD hebt die u meer controle over het verificatiebeleid voor voor de toepassing. Zie [access regels configureren voor](active-directory-conditional-access-azuread-connected-apps.md)meer informatie over voorwaardelijke toegang en het beschermen van apps met meerledige verificatie (MVR gesloten).

Uw toepassing Azure Active Directory registreren. Registreren van de toepassing betekent dat uw ontwikkelaars Azure AD verificatie van gebruikers en het aanvragen van toegang tot Gebruikersbronnen zoals e-mail, agenda en documenten kunnen gebruiken.

Elk lid van de directory (niet de gasten) kan een toepassing, ook bekend als het *maken van een application-object*te registreren.

Registreren van een toepassing, kan elke gebruiker het volgende doen:

- Ophalen van een identiteit voor de toepassing die door AD Azure herkend
- Een of meer geheimen/sleutels ophalen waarmee de toepassing zelf worden geverifieerd bij AD
- Merk de toepassing in de Azure portal met een aangepaste naam, logo, enz.
- Azure AD vergunning eigenschappen toegepast op de app, met inbegrip van:
  - Role-Based Access Control RBAC)
  - Azure Active Directory als oAuth vergunning server (een API die door de toepassing beveiligen)

- Nodig zijn voor de toepassing werkt zoals verwacht, met inbegrip van de vereiste machtigingen declareren:-App-machtigingen (alleen globale beheerders). Bijvoorbeeld: lidmaatschap van de rol in een andere Azure AD toepassing lidmaatschap of de rol ten opzichte van een Azure Resource, resourcegroep, of een abonnement - overgedragen machtigingen (een willekeurige gebruiker). Bijvoorbeeld: Azure AD, aanmelden en profiel lezen


> [AZURE.NOTE]Standaard kan een lid een toepassing registreren. Zie informatie over het beperken van machtigingen voor het registreren van toepassingen aan bepaalde leden, [hoe toepassingen worden toegevoegd aan AD Azure](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Hier ziet u de algemene beheerder moet doen om te helpen ontwikkelaars maken hun toepassing gereed is voor productie:

- Configureren van regels voor access (access policy/MVR gesloten)
- De app vereist de gebruiker is toegewezen en gebruikers toewijzen als u wilt configureren
- De voorziening toestemming onderdrukken

## <a name="configure-access-rules"></a>Toegangsregels voor configureren

Regels per toepassing toegang tot uw SaaS-toepassingen configureren. U kunt bijvoorbeeld MVR gesloten vereisen of toestaan alleen toegang aan gebruikers op vertrouwde netwerken. De details voor deze zijn beschikbaar in het document [toegangsregels configureren](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>De app vereist de gebruiker is toegewezen en gebruikers toewijzen als u wilt configureren

Standaard kunnen gebruikers toepassingen openen zonder dat wordt toegewezen. Als de toepassing ook rollen of als u wilt dat de toepassing moet worden weergegeven in het deelvenster voor toegang van een gebruiker, moet u Gebruikerstoewijzing vereisen.

[Toewijzing van gebruiker vereist](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Als u een abonnee Azure AD Premium of Enterprise Mobility Suite (EMS), wordt aangeraden met behulp van groepen. Groepen toewijzen aan de toepassing, kunt u delegeren continue toegangsbeheer aan de eigenaar van de groep. U kunt de groep maken of vraagt de verantwoordelijke partij in uw organisatie voor het maken van de groep met de groep management faciliteit.

[Gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md)  
[Groepen toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Toestemming van de gebruiker onderdrukken

Standaard wordt elke gebruiker een ervaring toestemming aan te melden. De ervaring is toestemming vragen van gebruikers machtigen om een toepassing, kan worden toegevoegd voor gebruikers die niet bekend zijn met het nemen van dergelijke beslissingen.

Voor toepassingen die u vertrouwt, kunt u de gebruikerservaring vereenvoudigen door stemt ermee in dat de toepassing voor uw organisatie.

Zie voor meer informatie over de toestemming van de gebruiker en de ervaring van toestemming in Azure, [Integratie van toepassingen met Azure Active Directory](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Verwante artikelen

- [Veilige externe toegang tot toepassingen voor ruimten met Azure AD toepassingsproxy inschakelen](active-directory-application-proxy-get-started.md)
- [Azure voorwaardelijke toegang-voorbeeld voor SaaS-toepassingen](active-directory-conditional-access-azuread-connected-apps.md)
- [Toegang tot apps met Azure Active Directory beheren](active-directory-managing-access-to-apps.md)
- [Artikel-Index voor Toepassingsbeheer in Azure Active Directory](active-directory-apps-index.md)

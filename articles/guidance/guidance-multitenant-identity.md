<properties
   pageTitle="Identity Management for Multitenant toepassingen | Microsoft Azure"
   description="Aanbevolen procedures voor verificatie, autorisatie en identiteit management bij multitenant-toepassingen."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Identiteitsbeheer voor multitenant toepassingen in Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Deze serie bevat aanbevelingen voor multitenancy, bij gebruik van Azure AD voor verificatie en identity management.

Wanneer u een multitenant-toepassing maakt, beheert een van de eerste uitdagingen gebruikersidentiteiten, omdat elke gebruiker nu een huurder behoort. Bijvoorbeeld:

- Gebruikers zich aanmelden met de referenties van de organisatie.
- Gebruikers moeten toegang hebben tot van hun organisatie gegevens, maar niet de gegevens die behoort tot de andere huurders.
- Een organisatie kunt aanmelden voor de toepassing en vervolgens toepassingsrollen toewijzen aan de leden.

Azure Active Directory (AD Azure) heeft enkele geweldige functies die ondersteuning bieden voor al deze scenario's.

Bij deze reeks artikelen, hebben wij ook een volledige [implementatie van begin tot eind] gemaakt[ tailspin] van een multitenant app. De artikelen geven we aan het bouwen van de toepassing hebt geleerd. Zie de [toepassing onderzoeken uitvoeren](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md)om te beginnen met de toepassing.

Hier is de lijst met artikelen in deze serie:

- [Inleiding tot identiteitsbeheer voor multitenant toepassingen](guidance-multitenant-identity-intro.md)
- [Over de toepassing van Tailspin enquêtes](guidance-multitenant-identity-tailspin.md)
- [Verificatie met behulp van Azure AD en OpenID verbinden](guidance-multitenant-identity-authenticate.md)
- [Werken met identiteiten op basis van een claim](guidance-multitenant-identity-claims.md)
- [Aanmelding en onboarding pachters](guidance-multitenant-identity-signup.md)
- [Toepassingsrollen](guidance-multitenant-identity-app-roles.md)
- [Rol- en resource vergunning](guidance-multitenant-identity-authorize.md)
- [Een back-end web API beveiligen](guidance-multitenant-identity-web-api.md)
- [Toegangstokens voor OAuth2 in cache opslaan](guidance-multitenant-identity-token-cache.md)
- [Federeren met AD FS van een klant voor multitenant toepassingen in Azure](guidance-multitenant-identity-adfs.md)
- [Met behulp van bevestiging van de client access tokens ophalen van Azure AD](guidance-multitenant-identity-client-assertion.md)
- [Met behulp van sleutel kluis geheimen toepassing beveiligen](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps

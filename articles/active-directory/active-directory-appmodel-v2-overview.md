<properties
    pageTitle="v2.0 eindpunt overzicht | Microsoft Azure"
    description="Een inleiding tot het bouwen van apps met zowel Microsoft-Account en Azure Active Directory aanmelden."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Aanmelden bij Microsoft-Account & Azure AD-gebruikers in een enkele app

In het verleden is een app-ontwikkelaars die wilden ondersteunen Microsoft-accounts en Azure Active Directory integreren met twee afzonderlijke systemen vereist.  Nu hebben wij een nieuwe verificatie API versie waarmee u gebruikers met beide soorten accounts met de Azure AD systeem aanmelden.  Dit verificatiesysteem geconvergeerde staat bekend als **het eindpunt v2.0**.  Met het eindpunt v2.0 kunt een eenvoudige integratie u een doelgroep die miljoenen gebruikers met een account voor zowel persoonlijke als werk/school omvat bereiken.

Toepassingen die gebruikmaken van het eindpunt v2.0 kunnen REST API's van het [Microsoft Graph](https://graph.microsoft.io) en [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) met beide rekening ook verbruiken.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Aan de slag
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Kies uw favoriete platform in de onderstaande lijst om een app met onze open-source-bibliotheken en frameworks te bouwen.  Ook kunt u onze documentatie 2.0 OAuth en OpenID verbinding protocol & protocol zonder gebruik van een bibliotheek auth rechtstreeks berichten verzenden.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Wat is er nieuw
De informatie hier is handig bij het begrijpen van wat is en wat niet mogelijk is met het eindpunt v2.0.

- Informatie over de [typen toepassingen die u met het eindpunt v2.0 maken kunt](active-directory-v2-flows.md).
- Begrip van de [beperkingen, beperkingen en beperkingen](active-directory-v2-limitations.md) aan het eindpunt v2.0.
- We hebt onlangs ondersteuning voor [admin beperkt bereik](active-directory-v2-scopes.md) en het [verlenen van referenties OAuth2-client](active-directory-v2-protocols-oauth-client-creds.md)toegevoegd.  Probeer ze!

## <a name="reference"></a>Referentie
Deze koppelingen zijn handig voor het verkennen van het platform in de diepte:

- Build 2016: [aan de slag met Microsoft identiteiten: Enterprise Grade inloggen voor uw Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Help opvragen voor een stackoverloop met de [azure active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) of [adal](http://stackoverflow.com/questions/tagged/adal) tags.
- [v2.0 Protocol verwijzing](active-directory-v2-protocols.md)
- [tokenverwijzing v2.0](active-directory-v2-tokens.md)
- [Referentie-bibliotheek v2.0](active-directory-v2-libraries.md)
- [Scopes en toestemming voor het in het eindpunt v2.0](active-directory-v2-scopes.md)
- [De registratie van Microsoft App-Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API: naslag](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [De Microsoft Graph](https://graph.microsoft.io)
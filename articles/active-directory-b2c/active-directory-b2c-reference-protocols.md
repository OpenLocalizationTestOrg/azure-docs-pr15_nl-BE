<properties
    pageTitle="B2C Azure Active Directory | Microsoft Azure"
    description="Het bouwen van apps via de protocollen die worden ondersteund door Active Directory-B2C Azure."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Verificatieprotocollen

Azure Active Directory (AD Azure) B2C identiteit als een service voor uw toepassingen biedt door de ondersteuning van twee industriestandaardprotocollen: verbinding OpenID en OAuth 2.0. De service is compatibel met de standaarden, maar de twee implementaties van deze protocollen kunnen subtiele verschillen hebben.  De informatie in deze handleiding zijn nuttig voor u als u de code schrijft rechtstreeks verzenden en verwerken van HTTP-verzoeken, in plaats van met behulp van een open-source bibliotheek. Het is raadzaam om deze pagina te lezen, voordat u een Duik in de details van elk specifiek protocol. Maar als u al bekend met Azure AD B2C bent, gaat u rechtstreeks naar [de Naslaggidsen protocol](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>De grondbeginselen
Elke toepassing die gebruikmaakt van Azure AD B2C dient te worden geregistreerd in de B2C-map in de [portal Azure](https://portal.azure.com). Het registratieproces app verzamelt en een paar waarden worden toegewezen aan uw app:

- Een **Toepassings-ID** die een unieke identificatie van uw app.
- Een **Redirect URI** of de **pakket-id** kan worden gebruikt voor directe antwoorden terug naar uw app.
- Een paar andere scenario-specifieke waarden. Voor meer informatie over [het registreren van uw toepassing](active-directory-b2c-app-registration.md).

Na registratie van uw app communiceren met Azure AD door het verzenden van verzoeken naar het eindpunt v2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

In bijna alle OAuth en OpenID verbinding stromen zijn vier partijen die betrokken zijn bij de uitwisseling:

![Rollen OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- De **server van de vergunning** is het eindpunt Azure AD v2.0. Veilig worden verwerkt alles wat betrekking heeft op gegevens en gebruikers toegang. Ook de vertrouwensrelaties tussen de partijen in een stroom wordt omgegaan. Het is verantwoordelijk voor het controleren van de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en tokens worden uitgeven. Het is ook bekend als de id-provider.
- De **eigenaar van de bron** is meestal de eindgebruiker. Het is de partij die eigenaar is van de gegevens en zij is bevoegd om derden voor toegang tot deze gegevens of de resource.
- De **client OAuth** is uw app. Die wordt geïdentificeerd door de aanvraag-ID. Het is meestal de partij waarmee eindgebruikers werken. Ook vraagt deze tokens van de server van de vergunning. De eigenaar van de resource moet de machtiging client toegang tot de bron.
- De **bronserver** is waar de gegevens of de resource zich bevindt. Hiermee vertrouwt u de machtiging server veilig verifiëren en machtigen de OAuth-client. Toegangstokens aan toonder wordt gebruikt om ervoor te zorgen dat de toegang tot een bron kan worden verleend.

## <a name="policies"></a>Beleid
Hoewel, Azure AD B2C-beleid zijn de belangrijkste functies van de service. Azure AD B2C breidt de standaardprotocollen 2.0 OAuth en OpenID verbinding maken door de invoering van beleid. Deze toestaan Azure AD B2C veel meer dan eenvoudige verificatie en machtiging uitvoeren. Beleid beschreven volledig consument identiteit ervaringen, met inbegrip van de aanmelding, aanmelden en profiel bewerken. Beleid kunnen worden gedefinieerd in een administratieve UI. Ze kunnen worden uitgevoerd met behulp van een speciale queryparameter in HTTP-verificatieaanvragen. Beleid zijn niet standaard functies van 2.0 OAuth en OpenID aansluiten, dus u moet de tijd om te begrijpen. Raadpleeg de [Naslaggids Azure AD B2C-beleid](active-directory-b2c-reference-policies.md)voor meer informatie.

## <a name="tokens"></a>Tokens
De uitvoering Azure AD B2C van 2.0 OAuth en OpenID verbinding maakt uitgebreid gebruik van toonder-tokens, aan toonder tokens die worden weergegeven als JSON web tokens (JWTs). Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. De drager is een partij die het token kan presenteren. Azure AD een partij moet eerst worden geverifieerd voordat het een token aan toonder kan ontvangen. Maar als de vereiste stappen niet voor het beveiligen van de token in de overdracht en opslag zijn genomen, kunnen worden onderschept en gebruikt door een onbedoelde partij.

Sommige beveiligingstokens een ingebouwd mechanisme dat voorkomt onbevoegden dat via hen hebben, maar aan toonder tokens hebt dit mechanisme. Zij moeten worden vervoerd in een beveiligd kanaal, zoals transport layer security (HTTPS). Als een token aan toonder buiten een beveiligd kanaal wordt verzonden, kunnen een kwaadwillende partij een man-in-the-middle-aanval kunt gebruiken om het token verkrijgen en gebruiken om onbevoegd toegang krijgen tot een beveiligde bron. Dezelfde beveiligingsprincipes toepassen als aan toonder tokens worden opgeslagen of voor later gebruik opgeslagen. Controleer altijd of uw app verzendt en aan toonder tokens worden opgeslagen op een veilige manier.

Zie [RFC 6750 sectie 5](http://tools.ietf.org/html/rfc6750)voor extra aan toonder token veiligheidskwesties.

Nadere gegevens over de verschillende soorten tokens die worden gebruikt in Azure AD B2C zijn beschikbaar in de [Naslaginformatie voor Azure AD token](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protocollen

Wanneer u bepaalde voorbeeld aanvragen bekijken, kunt u starten met een van de volgende zelfstudies. Elk een komt overeen met een bepaalde verificatie scenario. Hulp om te bepalen welke stroom geschikt voor u is, Bekijk [dat de soorten apps kunt bouwen met behulp van Azure AD B2C](active-directory-b2c-apps.md).

- [Mobiele en eigen toepassingen bouwen met OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Web apps bouwen met behulp van verbinding OpenID](active-directory-b2c-reference-oidc.md)

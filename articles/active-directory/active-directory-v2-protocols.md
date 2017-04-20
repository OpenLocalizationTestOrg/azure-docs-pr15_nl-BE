<properties
    pageTitle="Azure AD v2.0 protocollen | Microsoft Azure"
    description="Een handleiding voor de protocollen die worden ondersteund door het eindpunt Azure AD v2.0."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0 protocollen - 2.0 OAuth en OpenID verbinding

Het eindpunt v2.0 kunt Azure AD voor identiteit-as-a-service met industriestandaardprotocollen, verbinding OpenID en OAuth 2.0 gebruiken.  De service is compatibel met de standaarden, kunnen er subtiele verschillen tussen de twee implementaties van deze protocollen.  De informatie hier zijn handig als u schrijft de code rechtstreeks verzenden en verwerken van HTTP-aanvragen of gebruik een 3de partij open source bibliotheek in plaats van met een van onze open-source-bibliotheken.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Niet alle scenario's Azure Active Directory en functies worden ondersteund door het eindpunt v2.0.  Om te bepalen als u het eindpunt v2.0 moet gebruiken, Lees over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>De grondbeginselen
In bijna alle OAuth en OpenID verbinding stromen zijn er vier partijen die betrokken zijn bij de uitwisseling:

![Rollen OAuth 2.0](../media/active-directory-v2-flows/protocols_roles.png)

- De **Server van de vergunning** is het eindpunt v2.0.  Het is verantwoordelijk voor de waarborging van de identiteit van de gebruiker, verlenen en intrekken van toegang tot bronnen en tokens worden uitgeven.  Wordt ook wel de identiteitsprovider - niets te maken met gegevens van de gebruiker, de toegang en de vertrouwensrelaties tussen de partijen in een stroom veilig worden verwerkt.
- De **Eigenaar van de bron** is meestal de eindgebruiker.  De partij die eigenaar is van de gegevens en de macht bezit om het toestaan van derden toegang krijgen tot deze gegevens of de resource is.
- De **OAuth Client** is uw app, aangeduid met de toepassings-id.  Is meestal de partij die de eindgebruiker met samenwerkt en tokens van de server toestemming vraagt.  De client moet worden gemachtigd voor toegang tot de bron door de eigenaar van de resource.
- De **Bronserver** is waar de gegevens of de resource zich bevindt.  De Server toestemming voor het veilig verifiëren en machtigen van de OAuth Client vertrouwt en aan toonder access_tokens gebruikt om ervoor te zorgen dat de toegang tot een bron kan worden verleend.


## <a name="app-registration"></a>App registratie
Elke toepassing die gebruikmaakt van het eindpunt v2.0 moet op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) worden geregistreerd voordat OAuth of OpenID verbinding kan communiceren.  Het registratieproces app zal verzamelen & een paar waarden toewijzen aan uw app:

- Een **Toepassings-Id** die een unieke identificatie van uw app
- Een **Redirect URI** of de **Pakket-id** kan worden gebruikt voor directe antwoorden terug naar uw app
- Een paar andere scenario-specifieke waarden.

Informatie voor meer informatie over het [registreren van een app](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Eindpunten
Zodra geregistreerd, communiceert de app Azure AD door het verzenden van verzoeken naar het eindpunt v2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Waar de `{tenant}` kunt kiezen uit vier verschillende waarden krijgen:

| Waarde | Beschrijving |
| ----------------------- | ------------------------------- |
| `common` | Gebruikers met accounts van werk/school en persoonlijke accounts van Microsoft staat van Azure Active Directory wilt aanmelden bij de toepassing. |
| `organizations` | Alleen gebruikers met accounts van werk/school staat van Azure Active Directory wilt aanmelden bij de toepassing. |
| `consumers` | Kunnen alleen gebruikers met een persoonlijke Microsoft-account (MSA) in de toepassing ondertekenen. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`of`contoso.onmicrosoft.com` | Alleen gebruikers met accounts van werk/school staat van een bepaalde huurder Azure Active Directory wilt aanmelden bij de toepassing.  De gebruiksvriendelijke naam van de pachter Azure AD of guid-id van de huurder worden gebruikt.  |

Kies een bepaalde app type hieronder voor meer informatie over het werken met twee eindpunten.

## <a name="tokens"></a>Tokens
De uitvoering v2.0 van 2.0 OAuth en OpenID verbinding maken intensief gebruik van toonder-tokens, aan toonder tokens weergegeven als JWTs. Een token aan toonder is een lichtgewicht beveiligingstoken dat door de "houder" toegang tot een beveiligde bron verleent. In dit opzicht is de "houder" een partij die het token kan presenteren. Als een partij moet eerst worden geverifieerd met Azure Active Directory voor het ontvangen van de token aan toonder als worden niet de vereiste stappen voor het beveiligen van de token in de overdracht en opslag is genomen, kan het worden onderschept en gebruikt door een onbedoelde partij. Sommige beveiligingstokens beschikken over een ingebouwd mechanisme om te voorkomen dat onbevoegden kunnen gebruiken, kunnen aan toonder tokens hoeft dit mechanisme niet en moeten worden vervoerd in een beveiligd kanaal zoals transport layer security (HTTPS). Als een token aan toonder wordt verzonden in de Open, kan een man in het midden aanval worden gebruikt door een kwaadwillende partij de token verkrijgen en het gebruik voor een onbevoegde toegang tot een beveiligde bron. Dezelfde beveiligingsprincipes van toepassing wanneer of aan toonder tokens voor later gebruik opslaan in de cache opslaan. Controleer altijd of uw app verzendt en aan toonder tokens worden opgeslagen op een veilige manier. Zie voor meer overwegingen over de beveiliging op tokens aan toonder, [RFC 6750 deel 5](http://tools.ietf.org/html/rfc6750).

Meer informatie over de verschillende soorten tokens die worden gebruikt in het eindpunt v2.0 is beschikbaar in [de v2.0 token-eindpuntreferentie](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protocollen

Als u bepaalde aanvragen voorbeeld zien, aan de slag met een van de onderstaande zelfstudies.  Elk een komt overeen met een bepaalde verificatie scenario.  Als hulp om te bepalen wat de juiste stroom voor u is, kijk dan eens [de typen toepassingen die u met de versie 2.0 maken kunt](active-directory-v2-flows.md).

- [Mobile en de oorspronkelijke toepassing met OAuth 2.0 bouwen](active-directory-v2-protocols-oauth-code.md)
- [Build Web Apps met Open ID verbinding](active-directory-v2-protocols-oidc.md)
- [Bouwen van Apps in één pagina met de impliciete stroom OAuth 2.0](active-directory-v2-protocols-implicit.md)
- [Build Daemons of processen op de Server met de clientreferenties OAuth 2.0 Flow](active-directory-v2-protocols-oauth-client-creds.md)
- Tokens krijgen in een Web-API met de OAuth 2.0 namens Flow (binnenkort beschikbaar)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 

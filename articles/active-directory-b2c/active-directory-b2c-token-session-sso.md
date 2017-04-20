<properties
    pageTitle="Azure Active Directory B2C: Token, sessie en configuratie van single sign-on | Microsoft Azure"
    description="Token, sessie en één aanmelding-configuratie in Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, sessie en enkele aanmelding-configuratie

Deze functie kunt u nauwkeurig bepalen, op [basis van per beleid](active-directory-b2c-reference-policies.md)van:
 
1. Levensduur van beveiligingstokens uitgestoten door B2C Azure Active Directory (AD Azure).
2. De levensduur van sessies van een web toepassing beheerd door Azure AD B2C.
3. Eenmalige aanmelding (SSO) het gedrag voor meerdere toepassingen en beleid in de B2C-huurder.

U kunt deze functie gebruiken in uw huurder B2C als volgt:

1. Volg deze stappen om te [Navigeren naar de blade B2C-functies](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) op de Azure portal.
2. Klik op **beleid voor aanmelden**. *Opmerking: U kunt deze functie op elk beleidstype niet alleen op* *-In beleid***.
3. Een beleid openen door erop te klikken. Klik bijvoorbeeld op in **B2C_1_SiIn**.
4. Klik op **bewerken** op de bovenkant van het blad.
5. Klik op het **Token, sessie & single sign-on-config**.
6. Breng de gewenste wijzigingen aan. Meer informatie over de beschikbare eigenschappen in de volgende secties.
7. Klik op **OK**.
8. Klik op **Opslaan** boven aan het blad.

![Screenshot van token, sessie en single sign-on-config](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Levensduur van token-configuratie

Azure AD B2C ondersteunt het [OAuth 2.0 authorization protocol](active-directory-b2c-reference-protocols.md) voor het inschakelen van beveiligde toegang tot beveiligde bronnen. Voor het implementeren van deze ondersteuning, zendt Azure AD B2C verschillende [beveiligingstokens](active-directory-b2c-reference-tokens.md). Dit zijn de eigenschappen die u gebruiken kunt voor het beheren van de levensduur van beveiligingstokens uitgestoten door Azure AD B2C:

- **Toegang & ID, token-levensduur (minuten)**: de levensduur van het token OAuth 2.0 aan toonder toegang te krijgen tot een beveiligde bron gebruikt. ID-tokens problemen Azure AD B2C op dit moment. Deze waarde zou van toepassing op toegangstokens, ook wanneer we deze toe te voegen.
   - Standaard = 60 minuten.
   - Minimum (inclusief) = 5 minuten.
   - Maximaal (inclusief) = 1440 minuten.
- **Levensduur van tokens vernieuwen (dagen)**: de maximale periode waarvoor een token vernieuwen kan worden gebruikt voor het verkrijgen van een nieuwe- of id-token (en eventueel een nieuwe vernieuwen token, als uw toepassing had gekregen de `offline_access` bereik).
   - Standaard = 14 dagen.
   - Minimum (inclusief) = 1 dag.
   - Maximaal (inclusief) = 90 dagen.
- **Vernieuwen token lifetime venster schuiven (dagen)**: nadat deze periode is verstreken voor de gebruiker wordt gedwongen opnieuw te verifiëren, ongeacht de geldigheidsduur van de meest recente vernieuwen token verkregen door de toepassing. Het kan alleen worden opgegeven als de switch is ingesteld op **dat wordt begrensd**. Deze moet groter dan of gelijk is aan de **levensduur van tokens vernieuwen (dagen)** waarde. Als de switch is ingesteld op **Unbounded**, niet kunt u een specifieke waarde bevatten.
   - Standaard = 90 dagen.
   - Minimum (inclusief) = 1 dag.
   - Maximaal (inclusief) = 365 dagen.

Dit zijn enkele van de use-cases die u kunt inschakelen met behulp van deze eigenschappen:

- Toestaan dat een gebruiker in een mobiele toepassing blijft voor onbepaalde tijd, zolang hij of zij voortdurend actief op de toepassing is. U kunt dit doen door de **vernieuwen verschuivende venster levensduur van tokens (dagen)** overschakelen naar **Unbounded** in uw beleid-in.
- Voldoen aan de bedrijfstak van de beveiliging en naleving van voorschriften door de juiste toegang token levensduur.

## <a name="session-configuration"></a>Configuratie sessie

Azure AD B2C ondersteunt het [verificatieprotocol OpenID verbinding](active-directory-b2c-reference-oidc.md) voor het inschakelen van beveiligde aanmeldingspagina voor webtoepassingen. Dit zijn de eigenschappen die u gebruiken kunt voor het beheren van de sessies van een toepassing:

- **Web app sessie-levensduur (minuten)**: de levensduur van de cookie Azure AD B2C-sessie opgeslagen in de browser van de gebruiker bij een geslaagde verificatie.
   - Standaard = 1440 minuten.
   - Minimum (inclusief) = 15 minuten.
   - Maximaal (inclusief) = 1440 minuten.
- **Web app sessietime-out**: als deze schakeloptie is ingesteld op **Absolute**, wordt de gebruiker gedwongen opnieuw worden geverifieerd na de periode die is opgegeven door de **Web app sessie-levensduur (minuten)** is verstreken. Als u deze schakeloptie **rollend** (de standaardinstelling), blijft de gebruiker ingelogd als de gebruiker continu actief in uw webtoepassing is.

Dit zijn enkele van de use-cases die u kunt inschakelen met behulp van deze eigenschappen:

- Voldoen aan de bedrijfstak van de beveiliging en naleving door de juiste toepassing websessie levensduur.
- Verplicht tot nieuwe verificatie na een ingestelde periode tijdens de interactie met een hoge beveiliging deel van uw webtoepassing. 

## <a name="single-sign-on-sso-configuration"></a>Configuratie voor eenmalige aanmelding (SSO)

Als er meerdere toepassingen en beleid in de huurder B2C, kunt u gebruikersinteracties overheen met de eigenschap **één configuratie voor eenmalige aanmelding** beheren. U kunt de eigenschap ingesteld op een van de volgende instellingen:

- **Huurder**: dit is de standaardinstelling. Met deze instelling kan meerdere toepassingen en beleid in uw huurder B2C delen dezelfde sessie van de gebruiker. Bijvoorbeeld nadat een gebruiker zich bij een toepassing aanmeldt, kan Contoso winkelen, hij of zij ook naadloos inloggen op een andere één, Contoso apotheek, bij de toegang tot het.
- **Toepassing**: Hiermee kunt u een gebruiker sessie uitsluitend voor een toepassing, onafhankelijk van andere toepassingen. Bijvoorbeeld, als u wilt dat de gebruiker aan te melden bij de apotheek Contoso (met dezelfde referenties), zelfs als hij of zij al is aangemeld bij het winkelen Contoso, een andere toepassing op dezelfde B2C pachters. 
- **Beleid**: Hiermee kunt u een gebruiker sessie uitsluitend voor een onafhankelijk van de toepassingen met behulp van het beleid. Bijvoorbeeld als de gebruiker is reeds ingelogd en een multi factor authentication (MVR gesloten) stap voltooid, kan hij of zij toegang worden verleend tot hogere beveiliging delen van meerdere toepassingen als de sessie is gebonden aan het beleid niet verloopt.
- **Uitgeschakeld**: dit zorgt ervoor dat de gebruiker uit te voeren via de gebruiker volledige rit op elke uitvoering van het beleid. Bijvoorbeeld, Hierdoor kunnen meerdere gebruikers aan uw toepassing (in een scenario van het gedeelde bureaublad) te melden, zelfs terwijl u één gebruiker blijft ingelogd gedurende de hele tijd.

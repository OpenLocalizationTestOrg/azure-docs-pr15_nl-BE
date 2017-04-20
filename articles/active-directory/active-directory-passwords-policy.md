<properties
    pageTitle="Beleid voor wachtwoorden en beperkingen in Azure Active Directory | Microsoft Azure"
    description="Beschrijving van het beleid dat van toepassing op wachtwoorden in Azure Active Directory, met inbegrip van toegestane tekens, lengte en vervaldatum"
  services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="password-policies-and-restrictions-in-azure-active-directory"></a>Beleid voor wachtwoorden en beperkingen in Azure Active Directory

Het wachtwoordbeleid en beschreven die zijn gekoppeld aan de gebruikersaccounts die zijn opgeslagen in de map Azure AD complexiteitsvereisten.

> [AZURE.IMPORTANT] **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).

## <a name="userprincipalname-policies-that-apply-to-all-user-accounts"></a>UserPrincipalName beleidsregels voor alle gebruikersaccounts

Elke gebruikersaccount die u moet zich aanmelden bij het systeem Azure AD-verificatie moet een unieke gebruiker UPN (User Principal Name) waarde van het kenmerk dat is gekoppeld aan die account hebben. De volgende tabel contouren het beleid dat van toepassing op zowel op-ruimten die afkomstig van Active Directory-gebruikersaccounts (gesynchroniseerd met de cloud) en cloud alleen gebruikersaccounts.

|   Eigenschap           |     UserPrincipalName eisen  |
|   ----------------------- |   ----------------------- |
|  Toegestane tekens    |  <ul> <li>A-Z</li> <li>-z </li><li>0 – 9</li> <li> . - \_ ! \# ^ \~</li></ul> |
|  Niet-toegestane tekens  | <ul> <li>Alle '@' teken dat niet de naam van het domein tussen.</li> <li>Geen punt '.' onmiddellijk voorafgaande aan de '@' symbool</li></ul> |
| Lengte beperkingen  |       <ul> <li>Totale lengte mag niet meer dan 113 tekens</li><li>64 tekens vóór de ‘@’ symbool</li><li>48 tekens na de ‘@’ symbool</li></ul>

## <a name="password-policies-that-apply-only-to-cloud-user-accounts"></a>Beleid voor wachtwoorden die alleen van toepassing op gebruikersaccounts cloud

De volgende tabel beschrijft de instellingen voor wachtwoordbeleid beschikbaar die kunnen worden toegepast op de gebruikersaccounts die zijn gemaakt en beheerd in Azure AD.

|  Eigenschap       |    Vereisten          |
|   ----------------------- |   ----------------------- |
|  Toegestane tekens   |   <ul><li>A-Z</li><li>-z </li><li>0 – 9</li> <li>@# $ % ^ & * - _ ! + = [] {} & #124; \ : ‘ , . ? / ` ~ “ ( ) ;</li></ul> |
|  Niet-toegestane tekens   |       <ul><li>Unicode-tekens</li><li>Spaties</li><li> **Alleen sterke wachtwoorden**: mag een teken punt '.' onmiddellijk voorafgaande aan de '@' symbool</li></ul> |
|   Wachtwoordbeperkingen | <ul><li>8 tekens minimaal en maximaal 16 tekens</li><li>**Alleen sterke wachtwoorden**: vereist 3 op 4 van de volgende handelingen uit:<ul><li>Kleine letters</li><li>Tekens in hoofdletters</li><li>Cijfers (0-9)</li><li>Symbolen (Zie bovenstaande wachtwoordbeperkingen)</li></ul></li></ul> |
| Duur van wachtwoord vervallen      | <ul><li>Standaardwaarde: **90** dagen </li><li>Waarde kan worden geconfigureerd met de cmdlet Set-MsolPasswordPolicy uit de Azure Active Directory-Module voor Windows PowerShell.</li></ul> |
| Kennisgevingen voor vervallen van wachtwoorden |  <ul><li>Standaardwaarde: **14** dagen (voordat het wachtwoord is verlopen)</li><li>Waarde kan worden geconfigureerd met de cmdlet Set-MsolPasswordPolicy.</li></ul> |
| Wachtwoord verloopt |  <ul><li>Standaardwaarde: **false** dagen (geeft aan dat wachtwoord verloopt en is ingeschakeld) </li><li>Waarde kan worden geconfigureerd voor afzonderlijke gebruikersaccounts met de cmdlet Set-MsolUser. </li></ul> |
|  Wachtwoordgeschiedenis  | Laatste wachtwoord worden niet opnieuw gebruikt. |
|  Wachtwoord geschiedenis duur | Altijd |
|  Accountvergrendeling | Na 10 mislukte aanmeldingspogingen (onjuiste wachtwoord), wordt de gebruiker voor één minuut worden vergrendeld. Verdere wordt onjuiste aanmeldingspogingen vergrendeld de gebruiker om de duur te vergroten. |


## <a name="next-steps"></a>Volgende stappen

* **Weet u hier omdat u ondervindt problemen met inloggen?** In dat geval [hier hoe u kunt wijzigen en uw eigen wachtwoord opnieuw instellen](active-directory-passwords-update-your-own-password.md).
* [Uw wachtwoorden beheren vanaf elke locatie](active-directory-passwords.md)
* [De werking van wachtwoordbeheer](active-directory-passwords-how-it-works.md)
* [Aan de slag met wachtwoord Mangement](active-directory-passwords-getting-started.md)
* [Wachtwoordbeheer aanpassen](active-directory-passwords-customize.md)
* [Best Practices voor wachtwoord beheer](active-directory-passwords-best-practices.md)
* [Operationeel inzicht met wachtwoord Management rapporten ophalen](active-directory-passwords-get-insights.md)
* [Wachtwoordbeheer Veelgestelde vragen](active-directory-passwords-faq.md)
* [Problemen met wachtwoorden](active-directory-passwords-troubleshoot.md)
* [Meer informatie](active-directory-passwords-learn-more.md)

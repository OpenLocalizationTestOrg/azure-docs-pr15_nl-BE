<properties
    pageTitle="Azure Active Directory B2C: En beperkingen | Microsoft Azure"
    description="Een lijst met beperkingen met Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: En beperkingen

Er zijn verschillende functies en mogelijkheden van Azure Active Directory (AD Azure) B2C die nog niet ondersteund. Veel van deze bekende problemen en beperkingen wordt opgelost in de toekomst zullen, maar u moet zich bewust zijn van hen als u consument gerichte toepassingen met Azure AD B2C bouwt.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Problemen bij het maken van Azure AD B2C huurders

Als u problemen tijdens het [maken van een huurder Azure AD B2C ondervindt](active-directory-b2c-get-started.md), Zie [een huurder Azure AD of een huurder Azure AD B2C - problemen en oplossingen maken](active-directory-b2c-support-create-directory.md) voor instructies.

Opmerking: Er zijn bekende problemen wanneer u een bestaande B2C huurder verwijderen en opnieuw met dezelfde domeinnaam maken. U moet een huurder B2C maken met een andere domeinnaam.

## <a name="note-about-b2c-tenant-quotas"></a>Opmerking over B2C huurder quota

Het aantal gebruikers in een huurder B2C is standaard beperkt tot 50.000 gebruikers. Als u het quotum van de huurder B2C verhogen, moet u contact opnemen met ondersteuning.

## <a name="branding-issues-on-verification-email"></a>Huismerkgegevens op verificatie via e-mail

De standaard controle e-mail bevat Microsoft-huisstijl. Verwijderen wij deze in de toekomst. Nu kunt u deze verwijderen met de [huisstijl functie bedrijf](../active-directory/active-directory-add-company-branding.md).

## <a name="restrictions-on-applications"></a>Beperkingen voor toepassingen

De volgende typen toepassingen worden momenteel niet ondersteund in Azure AD B2C. Raadpleeg voor een beschrijving van de ondersteunde typen toepassingen [Azure AD B2C: typen toepassingen](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Toepassingen van één pagina (JavaScript)

Veel moderne toepassingen hebben een enkele pagina toepassing (SPA) front-end die voornamelijk in JavaScript is geschreven en wordt vaak gebruikt een framework SPA zoals AngularJS, Ember.js, Durandal, enz. Deze overdracht is nog niet beschikbaar in Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Daemons / server-side toepassingen

Toepassingen die langdurige processen bevatten of die worden toegepast zonder de aanwezigheid van een gebruiker moeten ook toegang krijgen tot beveiligde bronnen, zoals Web-API's. Deze toepassingen kunnen verifiëren en tokens ophalen met behulp van de identiteit van de toepassing (in plaats van de gedelegeerde identiteit van de consument) in de [stroom van OAuth 2.0-client referenties](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Deze overdracht is nog niet beschikbaar in Azure AD B2C, zodat nu toepassingen tokens alleen nadat een consument interactief aanmelden stroom heeft plaatsgevonden.

### <a name="standalone-web-apis"></a>Standalone Web-API 's

In de B2C Azure AD hebt u de mogelijkheid om te [bouwen van een Web-API die is beveiligd met tokens OAuth 2.0](active-directory-b2c-apps.md#web-apis). Echter dat Web-API is alleen mogelijk om tokens te ontvangen van een client die de dezelfde id deelt Bouwen van een Web-API die toegankelijk is vanuit verschillende andere clients wordt niet ondersteund.

### <a name="web-api-chains-on-behalf-of"></a>Web-API ketens (op-rekening-Of)

Vele architecturen zijn een Web-API die een andere downstream Web API, beide beveiligd door Azure AD B2C aanroepen. In dit scenario is het gebruikelijk in native clients die beschikken over een Web API back-end die op zijn beurt een on line service van Microsoft zoals de Azure AD Graph API aanroept.

In dit scenario met gekoppelde Web API kan worden ondersteund met behulp van de subsidie OAuth 2.0 Jwt aan toonder referentie, ook bekend als de stroom op rekening-van. Echter, de stroom aan-rekening-Of is momenteel niet geïmplementeerd in de B2C Azure AD.

## <a name="restriction-on-libraries-and-sdks"></a>Beperking van de bibliotheken en SDK 's

De ondersteunde Microsoft-bibliotheken die Azure AD B2C is zeer beperkt op dit moment. We bieden ondersteuning voor .NET gebaseerd web apps en diensten, alsmede NodeJS web apps en services.  We hebben ook een voorbeeld van .NET clientbibliotheek bekend als de MSAL die met Azure AD B2C in Windows & andere .NET-toepassingen kan worden gebruikt.

Wij momenteel geen ondersteuning voor andere talen en platforms, waaronder iOS & Android bibliotheek.  Als u wenst te bouwen op een ander platform dan de hiervoor vermelde, raden we u aan met behulp van een SDK open source, verwijzen naar onze [2.0 OAuth en OpenID verbinding Protocol](active-directory-b2c-reference-protocols.md) indien nodig.  Azure AD B2C implementeert OAuth en OpenID verbinding maakt, waardoor het mogelijk is een algemene bibliotheek OAuth of OpenID verbinding gebruiken voor integratie.

Onze iOS & Android slag zelfstudies gebruiken open source-bibliotheken die we hebben getest op compatibiliteit met Azure AD B2C.  Alle onze quick start-zelfstudies zijn beschikbaar in onze sectie [aan de slag](active-directory-b2c-overview.md#getting-started) .

## <a name="restriction-on-protocols"></a>Beperking van de protocollen

Azure AD B2C ondersteunt verbinding OpenID en OAuth 2.0. Echter niet alle functies en mogelijkheden van elk protocol is geïmplementeerd. Voor een beter begrip van de reikwijdte van protocol ondersteunde functionaliteit in Azure AD B2C, Lees onze [verbinding OpenID en OAuth 2.0 protocol voor meer informatie](active-directory-b2c-reference-protocols.md). Ondersteuning van SAML en WS-Fed-protocol is niet beschikbaar.

## <a name="restriction-on-tokens"></a>Beperking van de tokens

Veel van de tokens die zijn uitgegeven door Azure AD B2C zijn als JSON Web Tokens of JWTs geïmplementeerd. Niet alle gegevens in de JWTs ("claims" genoemd) is echter vrij als deze zijn moet of ontbreken. Enkele voorbeelden zijn de 'sub' en 'preferred_username' claims.  Als de waarden, opmaak of betekenis van claims wijziging over tijd, tokens voor uw bestaande beleid onverlet blijven - kunt u vertrouwen op hun waarden in productie apps.  Als waarden worden gewijzigd, zal geven wij u de mogelijkheid om die wijzigingen te configureren voor elk van uw beleid.  Beter inzicht in de tokens dat momenteel door de Azure AD B2C-service lezen via onze [tokenverwijzing](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Beperking van geneste groepen

Lidmaatschap van geneste groepen worden niet in Azure AD B2C huurders ondersteund. Zijn we niet van plan deze functionaliteit toevoegen.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Beperking van de differentiële query functie op Azure AD Graph API

De [functie query differentiële op Azure AD Graph API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) wordt niet ondersteund in Azure AD B2C huurders. Dit is op onze routekaart op lange termijn.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Problemen met Gebruikersbeheer in Azure klassieke portal

B2C-functies zijn toegankelijk op de Azure portal. Echter, kunt u de Azure klassieke portal toegang krijgen tot andere functies van de huurder, inclusief het Gebruikersbeheer van de. Er zijn momenteel een aantal bekende problemen met Gebruikersbeheer (het tabblad **gebruikers** ) op de klassieke Azure portal:

- Een lokale account gebruiker (dat wil zeggen, een consument die zich met een e-mailadres en wachtwoord, of een gebruikersnaam en wachtwoord aanmeldt), komt niet het veld **Gebruikersnaam** overeen met de-in-id (e-mailadres of gebruikersnaam) die is gebruikt tijdens het aanmelden. Dit komt omdat het veld weergegeven in de klassieke Azure portal eigenlijk de UPN (User Principal Name) is, wordt niet gebruikt in B2C-scenario's. Als u wilt weergeven in de id-in van de lokale account, het gebruikersobject in [Graph Explorer](https://graphexplorer.cloudapp.net/)te vinden. Vindt u hetzelfde probleem met een sociale account gebruiker (dat wil zeggen, een consument die zich met Facebook, Google +, enz aanmeldt.), maar er is in dat geval geen id-in om te spreken van.

    ![Lokale account - UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Een gebruiker met lokale account kun je het kan niet een van de velden bewerken en wijzigingen opslaan op het tabblad **profiel** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Problemen met geïnitieerde admin-wachtwoord opnieuw instellen op de klassieke Azure portal

Als u het wachtwoord opnieuw instellen voor een lokaal verbruik op basis van een account op de klassieke Azure portal (met de opdracht **Wachtwoord opnieuw instellen** op het tabblad **gebruikers** ), die de consument niet mogelijk zijn of haar wachtwoord wijzigen op de volgende aanmelden, als u een teken omhoog of beleid, aanmelden en uw toepassingen wordt vergrendeld. Gebruik de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) opnieuw instellen van wachtwoord van de consument (zonder het vervallen van wachtwoorden) of een teken in het beleid in plaats van een teken omhoog of beleid aanmelden als tijdelijke oplossing.

## <a name="issues-with-creating-a-custom-attribute"></a>Problemen met het maken van een aangepast attribuut

Een [aangepast attribuut toegevoegd op de Azure portal](active-directory-b2c-reference-custom-attr.md) wordt niet onmiddellijk gemaakt in uw huurder B2C. U hebt aangepast kenmerk in ten minste één van uw beleid voor het gebruik in de B2C-huurder worden gemaakt en beschikbaar via Graph API.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Problemen met het verifiëren van een domein op de klassieke Azure portal

U kunt een domein met succes op de [Azure klassieke portal](https://manage.windowsazure.com/)momenteel niet controleren.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Problemen met inloggen met het beleid van de MVR gesloten in de Safari-browsers

Aanvragen die worden uitgevoerd met tussenpozen op Safari browsers met HTTP 400 (onjuiste aanvraag) fouten-in beleid (met de MVR gesloten ingeschakeld). Dit is vanwege beperkingen van de bestandsgrootte van Safari lage cookie. Er zijn enkele oplossingen voor dit probleem:

- Gebruik de 'aanmelden of aanmelden policy' in plaats van de 'sign in beleid'.
- Verminder het aantal **claims van toepassing** in het beleid wordt aangevraagd.

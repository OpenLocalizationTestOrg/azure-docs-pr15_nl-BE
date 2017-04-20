
<properties
    pageTitle="Azure Active Directory hybride identiteit overwegingen bij het ontwerp - voorschriften inzake de controle van de toegang bepalen | Microsoft Azure"
    description="Heeft betrekking op de pijlers van de identiteit en de toegangsvereisten voor bronnen voor gebruikers in een omgeving met hybride identificeren."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Access control vereisten voor uw identiteit hybride oplossing bepalen
Wanneer een organisatie hun identiteit hybride oplossing ontwerpt kunt ze ook deze verkoopkans bekijken de toegangsvereisten voor de bronnen die zij plannen beschikbaar maken voor gebruikers. De toegang tot de alle vier pijlers van de identiteit, die cross:

- Beheer
- Verificatie
- Autorisatie
- Controle

De volgende secties wordt uitgelegd hoe verificatie en autorisatie in meer informatie, controle en beheer deel uitmaken van de levenscyclus van hybride identiteit. Lees de [hybride identiteit taken bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) voor meer informatie over deze mogelijkheden.

>[AZURE.NOTE]
[De vier pijlers van identiteit - Identity Management in de leeftijd van hybride IT-](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) Lees voor meer informatie over elk van deze pijlers.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Er zijn verschillende scenario's voor verificatie en autorisatie, deze scenario's hebben specifieke eisen waaraan moeten worden voldaan door de hybride identiteit oplossing die u het bedrijf wilt nemen. Scenario's met betrekking tot Business to Business (B2B) communicatie kunnen een extra uitdaging voor IT-beheerders toevoegen omdat zij ervoor zorgen moet dat de verificatie en autorisatie methode die wordt gebruikt door de organisatie met hun zakelijke partners communiceren kan. Tijdens het ontwerpen voor de vereisten voor verificatie en autorisatie, ervoor te zorgen dat de volgende vragen worden beantwoord:

- Uw organisatie zal verifiëren en toestaan alleen gebruikers die zich bij hun identiteit managementsysteem?
 - Zijn er plannen voor B2B-scenario's?
 - Indien Ja, u al weet welke protocollen (SAML, OAuth, Kerberos, Tokens of certificaten) worden gebruikt voor het verbinden van beide bedrijven?
- De hybride identiteit oplossing die u gaat aannemen ondersteuning die protocollen gebruikt?

Een ander belangrijk punt te overwegen is waar de opslagplaats van de verificatie die wordt gebruikt door gebruikers en partners worden geplaatst en het moet worden gebruikt. Houd rekening met de volgende kern twee opties:
- Gecentraliseerde: in dit model referenties, beleid en beheer van de gebruiker kunnen worden gecentraliseerd in ruimten of in de cloud.
- Hybride: in dit model referenties, beleid en beheer van de gebruiker worden gecentraliseerd in gebouwen en een gerepliceerde in de cloud.

Welk model de organisatie zal vaststellen zijn afhankelijk van hun zakelijke behoeften, wilt u antwoord op de volgende vragen om te bepalen waar het identity management system wordt geplaatst en de beheerdersmodus te gebruiken:

- Uw organisatie er een identity management in gebouwen?
 - Zo ja, zij van plan te houden?
 - Zijn er eventuele verordening of naleving van de eisen die uw organisatie moet volgen die bepaalt waar de identity management-systeem moet zich bevinden?
- Maakt uw organisatie gebruik van eenmalige aanmelding voor apps die zich in ruimten of in de cloud?
 - Zo ja, is de vaststelling van een model van hybride identiteit invloed op dit proces?

## <a name="access-control"></a>Toegangsbeheer
Verificatie en autorisatie zijn hoofdelementen om toegang te krijgen tot bedrijfsgegevens via de validatie van de gebruiker, is het ook belangrijk dat u het toegangsniveau dat deze gebruikers zijn en over de bronnen die het beheer van het niveau van de beheerders toegang hebben. Uw identiteit hybride oplossing moet kunnen gedetailleerde toegang bieden tot bronnen, overdracht en rollen gebaseerde toegangscontrole. Zorg ervoor dat de volgende vraag worden beantwoord met betrekking tot de toegang beheren:

- Heeft uw bedrijf meer dan één gebruiker met verhoogde bevoegdheden voor het beheren van uw identiteit
 - Indien Ja, elke gebruiker nodig heeft hetzelfde toegangsniveau?
- Moet uw bedrijf toegang aan gebruikers voor het beheren van bepaalde bronnen overdragen?
 - Zo ja, hoe vaak dit gebeurt?
- Uw bedrijf moet voor de integratie van functies van access control tussen lokale en cloud resources?
- Moet uw bedrijf toegang tot bronnen op basis van bepaalde voorwaarden te beperken?
- Zou uw bedrijf hebben voor elke toepassing die moet worden aangepast besturingselement toegang tot bepaalde bronnen?
 - Zo ja, waar deze apps staan (in ruimten of in de cloud)?
 - Zo ja, waar zijn die als doel bronnen (in ruimten of in de cloud)?

>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en voordelen/nadelen van elke optie.  Door deze vragen te beantwoorden zal u selecteren welke optie u best past bij uw zakelijke behoeften.

## <a name="next-steps"></a>Volgende stappen

[Beveiligingsplan vereisten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)

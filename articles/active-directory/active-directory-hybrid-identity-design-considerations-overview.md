<properties
    pageTitle="Azure Active Directory hybride identiteit overwegingen bij het ontwerp - overzicht | Microsoft Azure"
    description="Overzicht en gids voor overwegingen met betrekking tot de hybride identiteit inhoud in kaart"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory hybride identiteit ontwerpoverwegingen

Consument-apparaten worden proliferating de bedrijfswereld, en cloud-software as a service (SaaS) toepassingen eenvoudig vast te stellen. Als gevolg hiervan, controle van de toepassing toegang van gebruikers beheren voor interne datacenters en cloud platformen is het lastig om.  Van Microsoft identity oplossingen beslaan op gebouwen en cloud-gebaseerde mogelijkheden, maken van de identiteit van een gebruiker voor verificatie en machtiging tot alle bronnen, ongeacht de locatie. We noemen deze hybride identiteit. Er zijn verschillende ontwerpen en configuratieopties voor hybride identiteit met behulp van Microsoft-oplossingen, en in bepaalde gevallen moeilijk is om te bepalen welke combinatie beter voldoen aan de behoeften van uw organisatie. Deze gids voor overwegingen met betrekking tot de hybride identiteit helpt u te begrijpen hoe een hybride identiteit oplossing te ontwerpen die overeenkomt met het bedrijf en de behoefte aan technologie voor uw organisatie.  Deze handleiding wordt beschreven van een reeks stappen en de taken die u volgen kunt om een hybride identiteit oplossing ontwerpen die voldoet aan de unieke behoeften van uw organisatie. In de stappen en de taken geeft de handleiding de relevante technologieën en opties beschikbaar voor organisaties om te voldoen aan functionele en servicekwaliteit (zoals beschikbaarheid, schaalbaarheid, prestaties, beheerbaarheid en beveiliging) vereisten. Met name komt hybride identiteit ontwerp overwegingen gids de volgende vragen te beantwoorden: 

- Welke vragen moet ik vraag en antwoord om het station een hybride identiteit-specifieke ontwerp voor een probleem met de technologie of het domein dat het beste voldoet aan mijn eisen?
- Welke volgorde van activiteiten moet ik uitvoeren om een hybride identiteit oplossing voor het probleem met de technologie of domein ontwerpen 
- Welke hybride identiteit technologie en configuratie-opties zijn beschikbaar om te helpen me mijn voldoen? Wat zijn de e-mailverkeer tussen die opties zodat de beste optie voor mijn bedrijf selecteren?


## <a name="who-is-this-guide-intended-for"></a>Wie is deze handleiding bedoeld?
 CIO, CITO, Chief identiteit architecten, Enterprise architecten en IT-architecten die verantwoordelijk is voor het ontwerpen van een identiteit hybride oplossing voor middelgrote of grote organisaties.

## <a name="how-can-this-guide-help-you"></a>Hoe kan deze handleiding u helpen? 
Deze handleiding kunt u begrijpen hoe een hybride identiteit oplossing die kan een wolk op basis van identity management system integreren met uw huidige op ruimten identiteit oplossing te ontwerpen. De volgende afbeelding ziet u een voorbeeld van een hybride identiteit-oplossing, waarmee IT-beheerders voor het beheren van hun huidige oplossing voor Windows Server Active Directory integreren in ruimten met Microsoft Azure Active Directory kunnen gebruikers Single Sign On (SSO) alle toepassingen zich in de cloud en op ruimten bevinden.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


In de bovenstaande afbeelding is een voorbeeld van een hybride identiteit oplossing is gebruik te maken van cloud services integreren met mogelijkheden voor bedrijven om een enkele bron voor de eindgebruiker verificatieproces en ter vergemakkelijking van IT-resources te beheren. Hoewel dit heel gebruikelijk is, is identiteit van elke organisatie met hybride vormgeving waarschijnlijk anders dan in het voorbeeld in afbeelding 1 wordt geïllustreerd door verschillende vereisten. Deze handleiding bevat een reeks stappen en de taken die u volgen kunt om een hybride identiteit oplossing ontwerpen die voldoet aan de unieke behoeften van uw organisatie. In de volgende stappen en de taken, de handleiding voor de relevante technologieën en de beschikbare opties worden aan u gepresenteerd om te voldoen aan de functionele en service niveau kwaliteitseisen voor uw organisatie.

**Veronderstellingen**: U enige ervaring hebben met Windows Server, Active Directory Domain Services en Azure Active Directory. In dit document, nemen we aan dat u zijn op zoek naar hoe deze oplossingen kunnen voldoen aan de behoeften van uw bedrijf op hun eigen of in een geïntegreerde oplossing.

## <a name="design-considerations-overview"></a>Overzicht van overwegingen bij het ontwerp
Dit document bevat een aantal stappen en de taken die u volgen kunt om een hybride identiteit oplossing ontwerpen die best aan uw vereisten voldoet. De stappen worden weergegeven in een geordende reeks. Overwegingen bij het ontwerp u in de volgende stappen leert moet u mogelijk om te besluiten dat u hebt gemaakt in eerdere stappen echter vanwege conflicterende ontwerpkeuzen wijzigen. Elke poging is gedaan om u te waarschuwen voor potentiële conflicten van ontwerp in het hele document. 

U ontvangt tijdens het beste aan uw vereisten voldoet na het doorlopen van de stappen zo vaak als nodig is om op te nemen alle van de overwegingen in het document. 

| Hybride identiteit fase                                             | De lijst met onderwerpen                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Vereisten voor identiteit                                   | [Zakelijke voorwaarden bepalen](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Vereisten voor directory-synchronisatie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Meerledige verificatie eisen bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Een hybride identiteit aangenomen strategie definiëren](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Plan voor het verbeteren van de beveiliging van gegevens door middel van sterke identiteit oplossing | [Bepalen van vereisten voor de bescherming van gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Vereisten voor beheer van webinhoud bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Voorschriften inzake de controle van de toegang bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Beveiligingsplan vereisten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Strategie voor gegevensbescherming te definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plan voor hybride identity lifecycle                                | [Hybride identiteit beheertaken bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Beheer van synchronisatie](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hybride identiteit aangenomen strategie bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Download dit productoverzicht
U kunt een PDF-versie van de handleiding hybride identiteit overwegingen bij het ontwerpen van de [Technet-galerie](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288)downloaden. 

                                                             

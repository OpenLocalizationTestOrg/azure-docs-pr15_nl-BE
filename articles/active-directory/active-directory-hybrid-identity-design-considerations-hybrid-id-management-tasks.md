<properties
    pageTitle="Azure Active Directory hybride identiteit overwegingen bij het ontwerpen - hybride identiteit beheertaken bepalen | Microsoft Azure"
    description="Azure Active Directory gecontroleerd met voorwaardelijke toegang beheren, de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en voor het toestaan van toegang tot de toepassing. Zodra deze voorwaarden wordt voldaan, wordt de gebruiker geverifieerd en toegang hebben tot de toepassing."
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

# <a name="plan-for-hybrid-identity-lifecycle"></a>Plan voor hybride Identity Lifecycle 

Identiteit is een van de grondslagen van uw strategie voor enterprise mobility en toepassing. Uw identiteit is of u op naar uw mobiele apparaat of SaaS-toepassing ondertekent, de sleutel tot het verkrijgen van toegang tot alles. Een oplossing voor identiteitsbeheer omvat op het hoogste niveau te verenigen en synchroniseren tussen uw identiteit opslagplaatsen met automatiseren en centraliseren van het proces van het aanbod van middelen. De identiteit oplossing moet de identiteit van een gecentraliseerde via lokale en cloud en andere vorm van id-federatie ook gebruiken om gecentraliseerde verificatie onderhouden en veilig delen en samenwerken met externe gebruikers en bedrijven. Bronnen variëren van besturingssystemen en toepassingen voor mensen in of gekoppeld aan een organisatie. Organisatorische structuur kan worden gewijzigd ten behoeve van de provisioning beleidsmaatregelen en procedures.

Het is ook belangrijk dat u een oplossing afgestemd op uw gebruikers doordat ze zelf ervaringen houden ze productief. De oplossing voor uw identiteit is krachtiger als u kunt eenmalige aanmelding voor gebruikers in alle bronnen die ze nodig hebben op alle beheerders toegang niveaus gestandaardiseerde procedures kunnen gebruiken voor het beheer van gebruikersreferenties. Beheer op bepaalde niveaus kunnen worden verminderd of geëlimineerd, afhankelijk van de breedte van de oplossing voor het leveren van gegevens. Bovendien kunt u veilig verspreiden beheermogelijkheden, handmatig of automatisch tussen de verschillende organisaties. Een domeinbeheerder kan bijvoorbeeld dienen alleen de mensen en de bronnen in dat domein. Deze gebruiker inrichten en administratieve taken kunt uitvoeren, maar is niet geautoriseerd configuratie taken uitvoeren zoals het maken van werkstromen.


## <a name="determine-hybrid-identity-management-tasks"></a>Hybride identiteit beheertaken bepalen
Administratieve taken in uw organisatie te distribueren, verbetert de nauwkeurigheid en de doeltreffendheid van het beheer en verbetert het saldo van de werklast van een organisatie. Hier volgen de scharnierpennen die een robuuste identity managementsysteem definiëren.

 ![](./media/hybrid-id-design-considerations/Identity_management_considerations.png)


Hybride identiteit taken definiëren, moet u enkele essentiële kenmerken van de organisatie die de vaststelling van hybride identiteit te begrijpen. Het is belangrijk te begrijpen van de huidige opslaglocaties voor bronnen van de identiteit wordt gebruikt. Weet de core elementen, wordt het basissysteem eisen en op basis van dat moet u meer gedetailleerde vragen die tot een beter ontwerpbesluit voor de oplossing van uw identiteit leiden.  

Bij het definiëren van deze eisen, ervoor te zorgen dat ten minste de volgende vragen worden beantwoord.

- Configuratieopties: 
 - Ondersteunt de identiteit hybride oplossing een robuust toegangsbeheer en provisioning-systeem?
 - Hoe worden gebruikers, groepen en wachtwoorden te beheren gaan?
 - Is de identity lifecycle management reageert? 
      - Hoe lang duurt schorsing van de account wachtwoord updates?
      
- Licentiebeheer: 
 - Heeft de hybride oplossing grepen licentie identiteitsbeheer?
     - Indien Ja, welke mogelijkheden zijn beschikbaar?
- De oplossing wordt verwerkt in Licentiebeheer voor groep? 
      -Zo ja, is het mogelijk om een beveiligingsgroep toekennen? 
       -Zo ja, wordt de map wolk automatisch toegewezen licenties aan de leden van de groep? 
        -Wat gebeurt er als een gebruiker wordt vervolgens toegevoegd aan of verwijderd uit de groep, wordt een licentie worden automatisch toegewezen aan of verwijderd zo nodig? 

- Integratie met andere identiteitsproviders van derden:
- Kan deze hybride oplossing worden geïntegreerd met de van derden identiteitsproviders implementeren op?
- Is het mogelijk om alle andere identiteitsproviders verenigen in een samenhangende identiteit?
- Zo ja, hoe en waar zijn ze en welke mogelijkheden beschikbaar zijn?

## <a name="synchronization-management"></a>Beheer van synchronisatie
Een van de doelstellingen van een id-beheer kunnen doen om alle id-providers en bewaar deze gesynchroniseerd. U houdt de gegevens worden gesynchroniseerd op basis van een bindende master identiteitsprovider. In een hybride identiteit scenario, met een model voor het beheer van gesynchroniseerde alle gebruikers- en identiteiten beheren in een server op locatie en de rekeningen en, eventueel, de wachtwoorden naar de cloud synchroniseren. Hetzelfde wachtwoord op-pand invoert als hij of zij heeft in de cloud en bij het aanmelden, het wachtwoord door de oplossing van de identiteit is geverifieerd. Dit model maakt gebruik van een hulpprogramma directory synchronisatie.
 
![](./media/hybrid-id-design-considerations/Directory_synchronization.png)Juiste ontwerp de synchronisatie van uw identiteit hybride oplossing ervoor te zorgen dat de volgende vragen worden beantwoord: • wat zijn de sync-oplossingen beschikbaar zijn voor de identiteit hybride oplossing?
• Wat de eenmalige aanmelding mogelijkheden beschikbaar zijn?
• Wat zijn de opties voor id-federatie tussen B2B en B2C?

## <a name="next-steps"></a>Volgende stappen
[Hybride identiteit aangenomen strategie bepalen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)


## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)


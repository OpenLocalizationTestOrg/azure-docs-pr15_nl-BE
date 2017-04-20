<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - identiteit eisen vast | Microsoft Azure"
    description="Identificeer de zakelijke behoeften van het bedrijf dat u definieert de eisen voor het ontwerp van hybride identiteit zal leiden."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor identiteit voor uw identiteit hybride oplossing
De eerste stap bij het ontwerpen van een identiteit hybride oplossing is om te bepalen van de vereisten voor de bedrijfsorganisatie dat zal worden gebruik te maken van deze oplossing.  Hybride identiteit begint als een ondersteunende rol (ondersteunt alle cloud-oplossingen door middel van verificatie) en wordt nieuwe en interessante mogelijkheden die nieuwe werkbelasting voor gebruikers te ontgrendelen.  Deze workloads of diensten die u voor uw gebruikers wilt vaststellen, bepalen de eisen voor het ontwerp van hybride identiteit.  Deze services en werkbelasting moeten gebruikmaken van hybride identiteit beide ruimten op en in de cloud.  

U moet gaan over deze belangrijke aspecten van het bedrijf om te begrijpen wat het is nu een vereiste en het bedrijf plannen voor de toekomst. Als u de zichtbaarheid van de lange termijnstrategie voor hybride identiteit ontwerpen niet hebt, zijn er kans dat uw oplossing niet zal schaalbare de zakelijke behoeften groeien en wijzigen.   T hij diagram hieronder ziet u een voorbeeld van een hybride identiteit architectuur en de werklast die ontgrendeld zijn worden voor gebruikers. Dit is slechts een voorbeeld van de nieuwe mogelijkheden die kunnen worden ontgrendeld en geleverd met een effen hybride identiteit strategie. 
 
Sommige onderdelen die deel van de hybride identiteit architectuur uitmaken![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Zakelijke voorwaarden bepalen
Elk bedrijf heeft verschillende behoeften, zelfs als deze bedrijven deel uitmaken van dezelfde sector, de zakelijke vereisten kunnen variëren. U kunt nog steeds gebruikmaken van best practices van de industrie, maar uiteindelijk is de zakelijke behoeften van het bedrijf dat u definieert de eisen voor het ontwerp van hybride identiteit zal leiden. 

Zorg ervoor dat de volgende vragen om te bepalen van uw zakelijke behoeften beantwoorden:

- Is uw bedrijf op zoek naar operationele IT-kosten
- Is uw bedrijf op zoek naar beveiligde cloud activa (SaaS apps, infrastructuur)
- Is uw bedrijf op zoek naar uw IT te moderniseren
  - Uw gebruikers zijn mobiele en veeleisende IT voor het maken van uitzonderingen in de DMZ om verschillende type verkeer te krijgen tot andere bronnen?
  - Heeft uw bedrijf een verouderde toepassingen die nodig is om te worden gepubliceerd op deze moderne gebruikers, maar is niet eenvoudig te herschrijven?
  - Uw bedrijf nodig heeft om al deze taken uitvoeren en het onder controle te brengen op hetzelfde moment?
- Is uw bedrijf op zoek de gebruikers-id's beveiligen en risico's te beperken door nieuwe hulpprogramma's die gebruikmaken van de expertise van Microsoft Azure security expertise op bedrijfsterreinen
- Is uw bedrijf probeert te verwijderen van de gevreesde "externe" rekeningen in gebouwen en verplaats deze naar de cloud wanneer zij niet langer een bedreiging slapende binnen uw omgeving op ruimten zijn?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analyseren van lokalen identiteit infrastructuur
Nu u een idee met betrekking tot de zakelijke vereisten van uw bedrijf hebt, moet u uw identiteit op gebouwen infrastructuur evalueren. Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor de integratie van uw huidige identiteit oplossing naar de cloud identity managementsysteem. Zorg ervoor dat de volgende vragen beantwoorden:

- Welke oplossing voor verificatie en autorisatie biedt uw bedrijf op ruimten gebruiken? 
- Uw bedrijf er Synchronisatieservices op locatie?
- Uw bedrijf maakt gebruik van een identiteit leveranciers (IdP)?

Ook moet u rekening houden met de cloud services die uw bedrijf kan hebben. Uitvoeren van een beoordeling om te begrijpen van de huidige integratie met SaaS, IaaS of PaaS-modellen in uw omgeving is heel belangrijk. Zorg ervoor dat de volgende vragen beantwoorden tijdens deze beoordeling:
- Heeft uw bedrijf een integratie met een cloud serviceprovider
- Indien Ja, welke services worden gebruikt?
- Deze integratie in productie is of is het een leider?


>[AZURE.NOTE]
Als u niet beschikken over een nauwkeurige toewijzing van alle apps in uw cloud services, kunt u het hulpprogramma Discovery van Cloud App. Dit hulpprogramma kan uw IT-afdeling geven inzicht in de business van uw organisatie en consument cloud-apps. Die het eenvoudiger dan ooit om te ontdekken shadow IT in uw organisatie, met inbegrip van gegevens over de gebruikspatronen en alle gebruikers met toegang tot uw cloud-toepassingen. Toegang tot dit programma gaat u naar [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Identity Integratievereisten evalueren
Vervolgens moet u de identiteit integratie-eisen te evalueren. Deze evaluatie is belangrijk dat de technische voorschriften voor hoe gebruikers worden geverifieerd, de aanwezigheid van de organisatie zal uitzien in de cloud, hoe de organisatie vergunning wordt toegestaan en wat de gebruikerservaring is het verstandig om te definiëren. Zorg ervoor dat de volgende vragen beantwoorden:

- Uw organisatie gebruiken federation, standaard verificatie of beide?
- Federatie een vereiste is?  Vanwege het volgende:
 - Kerberos gebaseerde SSO
 - Uw bedrijf heeft een op gebouwen (een ingebouwd intern of 3de partij) toepassingen die gebruikmaakt van SAML of soortgelijke federation-mogelijkheden.
 - MVR gesloten via smartcards. RSA SecurID, enz.
 - Regels voor access client die het adres van de onderstaande vragen:
     1. Kan ik alle externe toegang tot Office 365 op basis van het IP-adres van de client blokkeren?
     1. Kan ik alle externe toegang tot Office 365, met uitzondering van Exchange ActiveSync blokkeren?
     1. Ik kan alle externe toegang tot Office 365, met uitzondering van de apps op basis van een browser (de OWA, gesimuleerde Productieorder) blokkeren
     1. Ik kan alle externe toegang tot Office 365 voor leden van groepen van aangewezen AD blokkeren
- Beveiligingscontrole/betreft
- Bestaande investeringen in federatieve verificatie
- Welke naam wilt onze organisatie gebruiken voor onze domein in de cloud?
- Heeft de organisatie een aangepaste domein?
    1. Dit domein is openbaar en gemakkelijk controleerbare via DNS?
    1. Als dat niet het geval is, vervolgens hebt u een publiek domein die kunnen worden gebruikt voor het registreren van een alternatieve UPN in AD?
- De gebruiker-id's gelijk zijn voor weergave cloud? 
- Heeft de organisatie apps die integratie met cloud-diensten
- De organisatie beschikt over meerdere domeinen en ze allemaal standaard of federatieve verificatie gebruikt?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Toepassingen die worden uitgevoerd in uw omgeving evalueren
Nu u een idee hebt over uw locatie op en cloud infrastructuur, moet u voor de evaluatie van de toepassingen die worden uitgevoerd in deze omgevingen. Deze evaluatie is belangrijk voor het definiëren van de technische voorschriften voor de integratie van deze toepassingen naar de cloud identity managementsysteem. Zorg ervoor dat de volgende vragen beantwoorden:

- Waar wordt onze toepassingen live
- Gebruikers toegang tot toepassingen in gebouwen?  In de cloud? Of beide?
- Zijn er plannen om de werkbelasting van bestaande toepassingen te verplaatsen naar de cloud?
- Zijn er plannen voor het ontwikkelen van nieuwe toepassingen die zich bevinden in ruimten of cloud in de cloud die wordt gebruikt voor verificatie?

## <a name="evaluate-user-requirements"></a>Gebruikerseisen evalueren
U moet ook de gebruikerseisen te evalueren. Deze evaluatie is belangrijk voor het definiëren van de stappen die nodig zijn voor het in dienst nemen aan en het helpen van gebruikers als ze naar de cloud overstappen. Zorg ervoor dat de volgende vragen beantwoorden:

- Gebruikers toegang tot toepassingen in gebouwen?
- Gebruikers toegang tot toepassingen in de cloud?
- Hoe gebruikers meestal kan aanmelden bij hun omgeving in de lokalen?
- Hoe zullen gebruikers aanmelden naar de cloud?

>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Bepalen beveiligingsplan eisen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) gaat over de opties die beschikbaar zijn en voor-/ nadelen van elke optie.  Door hebben beantwoord deze vragen u selecteert welke optie u best past bij uw bedrijf nodig heeft.

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor directory-synchronisatie](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)

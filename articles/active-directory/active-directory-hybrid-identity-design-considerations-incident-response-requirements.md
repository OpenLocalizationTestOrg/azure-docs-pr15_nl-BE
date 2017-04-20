
<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - incident rResponse vereisten bepalen | Vereisten voor Microsoft Azure "
    description="Bepalen van de mogelijkheden voor bewaking en rapportage voor de oplossing van hybride identiteit die kan worden overgenomen door te voeren acties voor het identificeren en een mogelijke bedreigingen te verminderen"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor uw identiteit hybride oplossing beveiligingsplan bepalen

Grote of middelgrote organisaties waarschijnlijk heeft een [security incident response](https://technet.microsoft.com/library/cc700825.aspx) geïmplementeerd om IT dienovereenkomstig te handelen op het niveau van het incident. Het identity management system is een belangrijk onderdeel van het responsproces incident omdat kan worden gebruikt om te helpen identificeren die een specifieke actie ten opzichte van het doel uitgevoerd. De hybride identiteit oplossing moet kunnen bieden mogelijkheden voor bewaking en rapportage die kunnen worden gebruikt door te voeren acties voor het identificeren en beperken van een mogelijke bedreiging. In een typische beveiligingsplan hebt u de volgende stappen als onderdeel van het plan:

1.  Eerste beoordeling.
2.  Incident communicatie.
3.  Schade-beheersing en vermindering van de risico's.
4.  Identificatie van wat het compromis en ernst was.
5.  Bewaring van bewijsmateriaal.
6.  Kennisgeving aan de betrokken partijen.
7.  Herstel van het systeem.
8.  Documentatie.
9.  Beoordeling van de schade en de kosten.
10. Proces en planning revisie.

Tijdens de identificatie van dit compromis en ernst fase, worden voor het identificeren van de systemen die zijn aangetast, bestanden die zijn geopend en de gevoeligheid van de desbetreffende bestanden bepalen. Uw identiteit hybride systeem moet mogelijk zijn om te voldoen aan deze vereisten, kunt u de identificatie van de gebruiker die deze wijzigingen hebt aangebracht. 

## <a name="monitoring-and-reporting"></a>Bewaking en rapportage
Vele malen de identiteit system kunt ook in de fase van de eerste beoordeling hoofdzakelijk als het systeem is ingebouwd in de controle- en rapportagemogelijkheden. IT-beheer moet worden herkend een verdachte activiteiten tijdens de eerste beoordeling of het systeem moet kunnen automatisch op basis van een vooraf geconfigureerde taak trigger. Veel activiteiten kunnen duiden op een mogelijke aanvalsvector is, maar in andere gevallen een onjuist geconfigureerd systeem zou kunnen tot een aantal valse meldingen in het inbraakdetectiesysteem leiden. 

Het identity managementsysteem moet helpen IT-beheerders om te identificeren en verdachte activiteiten te melden. Deze technische voorschriften kunnen gewoonlijk worden voldaan door monitoring van alle systemen en dat een verslaggevende mogelijkheid die potentiële bedreigingen kunt markeren. Met de onderstaande vragen kunt u het ontwerp van uw identiteit hybride oplossing waarbij rekening beveiligingsplan eisen rekening:

- Heeft uw bedrijf een security incident response in plaats?
 - Als Ja, de huidige identiteit managementsysteem gebruikt als onderdeel van het proces?
- Moet uw bedrijf identificeren verdachte aanmelding pogingen van gebruikers over de verschillende apparaten?
- Heeft uw bedrijf voor het detecteren van mogelijke gemanipuleerde gebruikersreferenties nodig?
- Heeft uw bedrijf controleren de toegang tot en de actie van de gebruiker nodig?
- Uw bedrijf nodig heeft om te weten wanneer een gebruiker zijn wachtwoord opnieuw instellen?

## <a name="policy-enforcement"></a>Afdwingen

Tijdens schade controle en vermindering van risico-fase is het belangrijk dat u snel de werkelijke en potentiële gevolgen van een aanval verminderen. Die actie die u gaat kunt nu het verschil tussen een klein en een groot incident maken. Het exacte antwoord is afhankelijk van uw organisatie en de aard van de aanval waarmee u wordt geconfronteerd. Als eerste beoordeling geconcludeerd dat een account is aangetast, moet u voor deze rekening wordt geblokkeerd afdwingen. Dat is slechts één voorbeeld waar de identity management-systeem zal worden gebruikt. Met de onderstaande vragen kunt u uw identiteit hybride oplossing ontwerpen rekening houdend hoe beleid wordt afgedwongen als u wilt reageren op een continue incident:

- Uw bedrijf heeft beleid in plaats voorkomen dat gebruikers van toegang tot het netwerk zo nodig?
 - Als u Ja kiest, de huidige oplossing integreren in hybride identity managementsysteem die u gaat aannemen?
- Moet uw bedrijf voorwaardelijke toegang voor gebruikers die in quarantaine afdwingen? 
 
>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Strategie bepalen voor gegevensbescherming](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en voordelen/nadelen van elke optie.  Door hebben beantwoord deze vragen u selecteert welke optie u best past bij uw bedrijf nodig heeft.

## <a name="next-steps"></a>Volgende stappen
[Strategie voor gegevensbescherming te definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)

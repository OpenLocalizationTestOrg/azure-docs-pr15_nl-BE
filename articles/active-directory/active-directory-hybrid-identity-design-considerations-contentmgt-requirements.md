<properties
    pageTitle="Azure Active Directory hybride identiteit ontwerpoverwegingen - vereisten voor beheer van webinhoud bepalen | Microsoft Azure"
    description="Biedt inzicht in de vereisten van het beheer van de inhoud van uw bedrijf bepalen. Meestal wellicht wanneer een gebruiker heeft zijn eigen apparaat hij ook meerdere referenties die door de toepassing waarin hij gebruikt zal worden afwisselend. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met persoonlijke referenties versus die gemaakt met behulp van referenties corporate. De oplossing voor uw identiteit moet kunnen werken met cloud services bieden een naadloze ervaring voor de eindgebruiker tijdens diens privacy garanderen en verhogen de beveiliging tegen lekkage bieden gegevens."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Vereisten voor uw identiteit hybride oplossing beheer van webinhoud bepalen

Vereisten van het beheer van de inhoud voor uw bedrijf kan direct invloed hebben op uw beslissing ten aanzien van welke identiteit hybride oplossing te gebruiken. Het bedrijf dient de eigen gegevens te beschermen met de verspreiding van meerdere apparaten en gebruikers de mogelijkheid om hun eigen apparaten ([BYOD](http://aka.ms/byodcg)), maar deze ook moet de privacy van gebruiker intact laten. Meestal wellicht wanneer een gebruiker heeft zijn eigen apparaat hij ook meerdere referenties die door de toepassing waarin hij gebruikt zal worden afwisselend. Het is belangrijk om te onderscheiden welke inhoud is gemaakt met persoonlijke referenties versus die gemaakt met behulp van referenties corporate. De oplossing voor uw identiteit moet kunnen werken met cloud services bieden een naadloze ervaring voor de eindgebruiker tijdens diens privacy garanderen en verhogen de beveiliging tegen lekkage bieden gegevens. 

De oplossing van uw identiteit wordt ontregeld raken verschillende technische controles om beheer van de inhoud zoals getoond in de onderstaande afbeelding:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Beveiliging besturingselementen die zal worden gebruik te maken van uw identity managementsysteem**

In het algemeen, vereisten voor beheer van webinhoud maken gebruik van uw identity management-systeem op de volgende gebieden:

- Privacy: identificatie van de gebruiker die eigenaar is van een bron en het toepassen van de juiste besturingselementen om integriteit te waarborgen.
- Data classificatie: identificatie van de gebruiker of groep en niveau van toegang tot een object volgens de classificatie. 
- Lekkage van gegevensbeveiliging: beveiliging besturingselementen die verantwoordelijk is voor de bescherming van gegevens om te voorkomen lekkage moet communiceren met de identiteit system valideren de identiteit van de gebruiker. Dit is ook belangrijk voor controledoeleinden trail.

>[AZURE.NOTE]
[Classificatie van gegevens voor cloud gereedheid](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) voor meer informatie over aanbevolen procedures en richtsnoeren voor de indeling van de gegevens lezen.

Als uw identiteit hybride oplossing planning zorgt ervoor dat de volgende vragen worden beantwoord volgens de vereisten van uw organisatie:

- Uw bedrijf beschikt over beveiliging besturingselementen voor het afdwingen van de gegevensprivacy?
 - Indien Ja, is deze beveiliging besturingselementen mogelijk te integreren met de hybride identiteit oplossing die u gaat aannemen?
- Uw bedrijf maakt gebruik van data classificatie?
 - Zo ja, kan de huidige oplossing integreren met de hybride identiteit oplossing die u gaat aannemen?
- Heeft uw bedrijf momenteel oplossing voor het lekken van gegevens? 
 - Zo ja, kan de huidige oplossing integreren met de hybride identiteit oplossing die u gaat aannemen?
- Uw bedrijf nodig heeft om toegang tot bronnen te controleren?
 - Indien Ja, welk type bronnen?
 - Zo ja, welk niveau van informatie is nodig?
 - Zo ja, waar het controlelogboek moet zich bevinden? In ruimten of in de cloud?
- Uw bedrijf nodig heeft voor het coderen van alle e-mailberichten met vertrouwelijke gegevens (SSNs, creditcardnummers, enzovoort)?
- Uw bedrijf nodig heeft voor het coderen van alle documenten/inhoud gedeeld met externe zakenpartners?
- Uw bedrijf nodig heeft voor het handhaven van bedrijfsbeleidsregels op bepaalde soorten e-mails (geen antwoord op alle handelingen, niet doorsturen)?
 
>[AZURE.NOTE]
Zorg ervoor dat de notities van elk antwoord en inzicht in de grondgedachte achter het antwoord. [Strategie voor gegevensbescherming definiëren](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) gaat over de beschikbare opties en voordelen/nadelen van elke optie.  Door hebben beantwoord deze vragen u selecteert welke optie u best past bij uw bedrijf nodig heeft.


## <a name="next-steps"></a>Volgende stappen
[Voorschriften inzake de controle van de toegang bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht van overwegingen bij het ontwerp](active-directory-hybrid-identity-design-considerations-overview.md)

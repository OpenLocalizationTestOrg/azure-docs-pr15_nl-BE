<properties 
    pageTitle="Azure Mobile Engagement uitvoering voor Gaming-App"
    description="Games app scenario voor het implementeren van Azure Mobile Engagement" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Mobiele betrokkenheid bij games App implementeren

## <a name="overview"></a>Overzicht

Het opstarten van een gaming heeft een nieuwe visserij gebaseerd role play/strategie spel app gelanceerd. Het spel is al uitgevoerd en 6 maanden. Dit spel is een groot succes en heeft miljoenen downloads en de handhaving is zeer hoog in vergelijking met andere spel apps opstarten. Tijdens de vergadering driemaandelijkse herziening overeenkomen belanghebbenden dat ze nodig hebben om de gemiddelde opbrengst per gebruiker (ARPU) te verhogen. Premium-game-pakketten zijn beschikbaar als speciale aanbiedingen. Deze game packs kunnen gebruikers om te upgraden, het uiterlijk en de prestaties van de vissen en lokmiddelen of tackles in het spel. Pakket verkoop zijn echter zeer gering. Dus zij eerst bepalen voor het analyseren van de gebruikerservaring met een analytics tool en voor het ontwikkelen van een afspraak Klik geavanceerde programma te verhogen met behulp van verkoop Segmentatie.

Gebaseerd op de [Azure Mobile Engagement - handleiding aan de slag met Best practices](mobile-engagement-getting-started-best-practices.md) bouwen ze een betrokkenheid bij de strategie.

##<a name="objectives-and-kpis"></a>Doelstellingen en KPI 's

Voldoen aan de voornaamste belanghebbenden voor het spel. Leven één hoofddoel - premium pakket verkoop verhogen met 15%. Ze maken deze doelstelling Business Key Performance Indicators (KPI's) te meten en station

* Op welk niveau van het spel zijn deze pakketten gekocht?
* Wat is de opbrengst per gebruiker, per sessie, per week en per maand?
* Wat zijn de favoriete aankoop typen?

Deel 1 van de [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) wordt uitgelegd hoe de doelstellingen en KPI's definiëren. 

Met de Business KPI's nu gedefinieerd, maakt de Mobile Product Manager KPI's Engagement om te bepalen van de nieuwe gebruiker trends en bewaren.

* Bewaken, bewaren en het gebruik in de volgende intervallen: per dag, 2 dagen, wekelijks, maandelijks en elke 3 maanden
* Telt het aantal actieve gebruiker
* De beoordeling van de app in de winkel

Op basis van de aanbevelingen van het IT-team, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is mijn gebruikerspad (welke pagina wordt bezocht, hoeveel gebruikers tijd besteden aan het)
* Aantal crashes of bugs gevonden per sessie
* Welke versies van het besturingssysteem worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Wat voor soort internetverbinding heb mijn gebruikers?

Voor elke KPI geeft het Product Mobile Manager de gegevens die ze nodig heeft en waar deze zich bevindt in haar playbook.

## <a name="engagement-program-and-integration"></a>Programma betrokkenheid en integratie

Voordat u een programma met geavanceerde betrokkenheid, moeten de mobiele Project directeur verantwoordelijk voor het project hebben een diep begrip van hoe en wanneer de producten worden verbruikt door de gebruikers.

Na 3 maanden, heeft de directeur van Mobile Project voldoende gegevens ter verbetering van zijn verkoop-app push meldingen verzameld. Hij leert dat:

* De eerste aankoop meestal gebeurt op het niveau van 14. Voor 90% van de gevallen is de opdracht nieuwe legendarische wapens voor $3.
* In 80% van de gevallen aankopen van gebruikers die een aankoop, gaat u verder met het product en breng meer hebt aangebracht.
* Gebruikers die het niveau van de 20, zijn geslaagd beginnen te besteden meer dan $10 per week.
* Gebruikers vaak kopen premium pakketten op niveau 16, 24 en 32.

Dankzij deze analyse besluit de mobiele Project directeur maken bepaalde push notification in app verkoop verhogen. Hij maakt drie push-reeksen die hij belt: Welkom programma en verkoop programma inactief wordt. Raadpleeg voor meer informatie de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->

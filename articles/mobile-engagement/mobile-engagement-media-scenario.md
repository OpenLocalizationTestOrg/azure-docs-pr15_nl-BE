<properties 
    pageTitle="Azure Mobile Engagement uitvoering voor Media App"
    description="Media app scenario voor het implementeren van Azure Mobile Engagement" 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Mobiele betrokkenheid bij Media App implementeren

## <a name="overview"></a>Overzicht

John is een mobiel projectmanager voor een groot media bedrijf. Hij onlangs een nieuwe toepassing die een aantal zeer hoge download heeft gestart. Hij heeft zijn doelstellingen voor download limiet, maar nog steeds zijn terug op Investment(ROI) per gebruiker niet voldoen aan zijn behoeften. 

John heeft al ontdekt waarom zijn ROI te laag is. Gebruikers vaak stoppen met zijn app na slechts twee weken en de meeste van hen nooit terugkomen. Hij wil verhogen, de handhaving van zijn app.

Nadat u enkele eerste testen, vaak dat hij heeft geleerd wanneer hij zijn gebruikers met push-meldingen, voert ze zijn app gebruiken. Ook retourneert gebruikers die niet actief zijn vaak afhankelijk van de meldingen hij deze verzendt App. John besluit om te investeren in een vorm van betrokkenheid bij de programma voor zijn toepassing die gebruikmaakt van geavanceerde gericht met push-meldingen.

John heeft onlangs de [Azure Mobile Engagement - handleiding aan de slag met Best practices](mobile-engagement-getting-started-best-practices.md) lezen en heeft besloten voor de uitvoering van de aanbevelingen uit de gids.

##<a name="objectives-and-kpis"></a>Doelstellingen en KPI 's

Voldoen aan de voornaamste belanghebbenden voor de toepassing van John. Als gebruikers zijn media verbruiken Business van advertenties gegenereerd. John verhoogt doordat inhoud verbruikt per gebruiker zijn inkomsten. Allemaal afspreken één hoofddoel: verkoop van advertenties met 25% verhogen. Ze maken deze doelstelling Business Key Performance Indicators (KPI's) te meten en station

* Aantal advertenties geklikt per gebruiker
* Hoeveel artikel pagina's bezocht (per gebruiker / per sessie / per week / maand...)
* Wat zijn favoriete rubrieken

Op basis van de vergadering met de voornaamste belanghebbenden van Jan heeft hij zijn Business KPI's gedefinieerd. Hij volgt deel 1 van de [Azure Mobile Engagement - handleiding aan de slag met Best practices](mobile-engagement-getting-started-best-practices.md). 

Hij maakt vervolgens de volgende Engagement KPI's om ervoor te zorgen dat de doelstellingen worden bereikt:

* Inhouding controleren via de volgende intervallen: dagelijkse, wekelijkse, tweewekelijkse en maandelijkse.
* Telt het aantal actieve gebruikers
* De beoordeling van de app in de app worden opgeslagen

Op basis van de aanbevelingen van het IT-team, zijn de volgende technische KPI's toegevoegd aan de volgende vragen beantwoorden:

* Wat is mijn gebruikerspad (welke pagina wordt bezocht, het aantal gebruikers van de tijd besteden aan het)
* Het aantal crashes of bugs gevonden per sessie?
* Welke versies van het besturingssysteem worden uitgevoerd voor Mijn gebruikers?
* Wat is de gemiddelde grootte van het scherm voor Mijn gebruikers?
* Wat voor soort internet-verbindingen hebt mijn gebruikers?

Hij classificeert de benodigde gegevens voor de KPI's, en hij op de juiste locatie van de playbook zijn vastgelegd.

## <a name="engagement-program-and-integration"></a>Programma betrokkenheid en integratie

Nu zijn KPI's definiëren, die John is voltooid, begint hij zijn Engagement strategie fase door 4 betrokkenheid bij de programma's en hun doelstellingen te definiëren:    ![][1]

John gaat vervolgens dieper door met informatie over push-meldingen voor elk programma. Push-bericht worden gedefinieerd door de vijf elementen:

1. Doelstelling: Wat is het doel van de melding
2. Hoe het doel wordt bereikt
3. Doel: wie de melding ontvangt?
4. Inhoud: Wat is de tekst en de opmaak van het bericht (In App/Out van App)
5. Wanneer: Wat is het beste moment om deze push-melding te verzenden

    ![][2]

Raadpleeg voor meer informatie de [hulpmiddelen marketing en verkoop](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Volgens het deel 2 van het witboek John doelsectie gebruikt om te bepalen welke hij heeft voor het verzamelen van gegevens en schrijft zijn label plannen samen met IT-team om de oplossing te implementeren. John na 1 week van uitvoering en de tests voor gebruikersaanvaarding kunt ten slotte zijn programma's starten.

##<a name="program-results"></a>Programma resultaten

4 maanden later, beoordeelt John de prestaties van programma's. Het Welkom-programma en het wekelijkse programma zijn doelstellingen zijn. Hiermee verlaagt u het aantal gebruikers met slechts één sessie, meer functies van de toepassing worden gebruikt en het aantal verbindingen per week is verdubbeld.

Het **Programma inactief** helpt John instinct gebruiker begrijpt. 15% van de inactieve gebruikers komen terug naar de toepassing wordt weergegeven. Maar de meeste van hen blijven niet actief meer dan 1 maand. John voorziet in een mogelijke optimalisatie van deze reeks met aanvullende kennisgevingen en zijn inhoud keuzen uit te vouwen.

Het **Programma Discover** werkt niet goed. Neemt cross-verkoopt maar niet genoeg om zijn doelstellingen te bereiken. John geeft aan dat hij geen voldoende gegevens om relevante aanwijzen en voorstellen van passende inhoud. Hij stopt met dit programma en is gericht op het verzenden van "redactionele push-meldingen' met Azure Mobile Engagement. De journalisten zijn al een CMS-oplossing om push-meldingen te verzenden en ze niet wilt wijzigen.

John besluit de bereiken API die een HTTP-REST API waarmee Reach campagnes beheren zonder gebruik te maken van AZME Web-interface wordt gebruikt. Met deze benadering kan John verzamelen de gegevens die hij nodig heeft en zijn schrijvers toestaan te houden met behulp van de CMS-oplossing.

John vraagt om ervoor te zorgen dat de functie goed werkt, IT-team waakzaam op de volgende punten:

1. **Besturingssystemen** : deze hebben allemaal hun eigen regels voor het beheren van push-meldingen, zodat John besluit om alle gevallen en gecontroleerd als de API's voor het verwerken.
Bijvoorbeeld: Android push-systeem kan grote lijnen die niet het geval bij iOS.

2. **Periode**: John wil een API die het tijdsbestek ingesteld en een einde aan campagnes. Hij wil dat gebruikers een melding storend bombing behouden.

3. **Categorieën**: Marketing-team bereidt de sjabloon voor elk type waarschuwing. John vraagt IT-team categorieën binnen de API instellen.

Na enkele tests is John tevreden. Dankzij deze API, kunnen journalisten blijven verzenden push-meldingen met de CMS- en Azure Mobile Engagement verzamelt gegevens voor alle doorgevoerd voor hen

Na deze 4 eerst maanden resultaten geven een goede algehele prestaties en biedt betrouwbaarheid voor Jan en zijn bestuur rendement per gebruiker verhogingen per 15% en mobiele verkoop 17,5% van de totale omzet, een stijging van 7,5% in slechts vier maanden vertegenwoordigen.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks

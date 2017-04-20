<properties
    pageTitle="AutoScaling en App-omgeving | Microsoft Azure"
    description="AutoScaling en App-omgeving"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"
/>

# <a name="autoscaling-and-app-service-environment"></a>AutoScaling en App-omgeving

Azure App Service omgevingen ondersteunen *autoscaling*. U kunt afzonderlijke werknemer van toepassingen automatisch schalen op basis van maatstaven of schema.

![Automatisch schalen de opties voor een groep werknemers.][intro]

AutoScaling optimaliseert het brongebruik door automatisch vergroten en verkleinen van een App-omgeving aanpassen aan uw budget en of profiel laden.

## <a name="configure-worker-pool-autoscale"></a>Automatisch schalen van toepassingen van werkprocessen configureren

Toegang tot de functionaliteit automatisch schalen uit het tabblad **Instellingen** van de groep werknemers.

![Het tabblad instellingen van de werknemer van toepassingen.][settings-scale]

Daar is dat de interface moet bekend zijn sinds het dezelfde manier dat u ziet wanneer u de schaal van een App serviceplan. 

![De instellingen handmatig schalen.][scale-manual]

U kunt ook een profiel automatisch schalen.

![Instellingen voor automatisch schalen.][scale-profile]

Automatisch schalen profielen zijn handig om beperkingen op de schaal. Op deze manier kunt u een consistente prestaties optreden door een ondergrens schaalwaarde (1) en een initiaal voorspelbare uitgaven worden door het instellen van een bovengrens (2) hebben.

![Schaalinstellingen voor in profiel.][scale-profile2]

Nadat u een profiel hebt gedefinieerd, kunt u de regels naar boven of beneden het aantal exemplaren in de groep van de werknemer binnen de grenzen die zijn gedefinieerd door het profiel te schalen automatisch schalen toevoegen. Automatisch schalen regels zijn gebaseerd op cijfers.

![Regel van de schaal.][scale-rule]

 Een werknemer van toepassingen of front-end metrics kan worden gebruikt om automatisch schalen regels te definiëren. Deze gegevens zijn dezelfde parameters kunt u controleren in de bron blade grafieken of waarschuwingen voor instellen.

## <a name="autoscale-example"></a>Voorbeeld van automatisch schalen

Automatisch schalen van een App-omgeving kan best worden geïllustreerd door stap voor stap een scenario.

In dit artikel worden de nodige overwegingen bij het instellen van automatisch schalen. Het artikel begeleidt u bij de interacties die een rol spelen als u autoscaling App Service-omgevingen die worden gehost in App-omgeving.

### <a name="scenario-introduction"></a>Scenario-Inleiding

Frank is een sysadmin voor een onderneming die een deel van de werklast die hij beheert is gemigreerd naar een App-omgeving.

De App-omgeving is geconfigureerd voor handmatig schalen als volgt:

* **Uiteinden van de voorkant:** 3
* **Werknemer-groep 1**: 10
* **Werknemer-groep 2**: 5
* **Werknemer groep 3**: 5

Werknemer pool 1 wordt gebruikt voor productie-werklasten en werknemer groep 2 en groep 3 met werknemer voor quality assurance (QA) en ontwikkeling werklasten worden gebruikt.

De App serviceplannen voor QA en dev zijn geconfigureerd voor handmatig schalen. De productie van App serviceplan is ingesteld op automatisch schalen omgaan met verschillen in belasting en verkeer.

Frank zeer vertrouwd is met de toepassing. Hij weet dat de piekuren voor belasting tussen 9:00 en 18:00 uur zijn omdat dit een line of business (LOB)-toepassing waarmee de medewerkers in het kantoor. Gebruik daalt daarna wanneer gebruikers klaar bent voor die dag. Buiten de piekuren nog zich steeds enkele laden omdat gebruikers toegang de app op afstand tot hebben met behulp van hun mobiele apparaten of thuis-pc's. De productie van App serviceplan is al geconfigureerd om automatisch schalen op basis van CPU-gebruik met de volgende regels:

![Specifieke instellingen voor LOB-toepassingen.][asp-scale]

|   **Profiel automatisch schalen: weekdagen – App serviceplan**     |   **Profiel automatisch schalen: weekend – App serviceplan**     |
|   ----------------------------------------------------    |   ----------------------------------------------------    |
|   **Naam:** Profiel van weekdag                               |   **Naam:** Weekend-profiel                               |
|   **Schalen door:** Planning- en regels            |   **Schalen door:** Planning- en regels            |
|   **Profiel:** Weekdagen                                   |   **Profiel:** Weekend                                    |
|   **Type:** Terugkeerpatroon                                    |   **Type:** Terugkeerpatroon                                    |
|   **Doelbereik:** 5-20 exemplaren                     |   **Het doelbereik:** 3-10 exemplaren                     |
|   **Dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag  |   **Dagen:** Zaterdag, zondag                              |
|   **Begintijd:** 9:00 uur                                 |   **Begintijd:** 9:00 uur                                 |
|   **Time zone:** UTC-08                                   |   **Time zone:** UTC-08                                   |
|                                                           |                                                           |
|   **De regel automatisch schalen (schaal van)**                           |   **De regel automatisch schalen (schaal van)**                           |
|   **Bron:** Productie (App-omgeving)      |   **Bron:** Productie (App-omgeving)      |
|   **Metric:** CPU-PERCENTAGE                                       |   **Metric:** CPU-PERCENTAGE                                       |
|   **Bewerking:** Meer dan 60%                         |   **Bewerking:** Meer dan 80%                         |
|   **Duur:** 5 minuten                                 |   **Duur:** 10 minuten                                |
|   **Tijd aggregatie:** Gemiddelde                           |   **Tijd aggregatie:** Gemiddelde                           |
|   **Actie:** Aantal verhogen door 2                         |   **Actie:** Telling verhoogd met 1                         |
|   **Afkoelen (minuten):** 15                             |   **Afkoelen (minuten):** 20                             |
|                                                           |                                                           |
  	|   **De regel automatisch schalen (schaal omlaag)**                     |   **De regel automatisch schalen (schaal omlaag)**                         |
|   **Bron:** Productie (App-omgeving)      |   **Bron:** Productie (App-omgeving)      |
|   **Metric:** CPU-PERCENTAGE                                       |   **Metric:** CPU-PERCENTAGE                                       |
|   **Bewerking:** Minder dan 30%                            |   **Bewerking:** Minder dan 20%                            |
|   **Duur:** 10 minuten                                |   **Duur:** 15 minuten                                |
|   **Tijd aggregatie:** Gemiddelde                           |   **Tijd aggregatie:** Gemiddelde                           |
|   **Actie:** Aantal verkleinen met 1                         |   **Actie:** Aantal verkleinen met 1                         |
|   **Afkoelen (minuten):** 20                             |   **Afkoelen (minuten):** 10                             |

### <a name="app-service-plan-inflation-rate"></a>App Service plan inflatie

App Service plannen die zijn geconfigureerd om automatisch schalen uit te doen met een maximale snelheid per uur. Dit percentage kan worden berekend op basis van de waarden die op de regel automatisch schalen.

Inzicht in en het berekenen van de *inflatie van App Service plan* is belangrijk voor de App Service omgeving automatisch schalen omdat de schaal wordt gewijzigd naar een groep werknemers niet onmiddellijk.

De inflatie App Service plan wordt als volgt berekend:

![App plan inflatie berekening.][ASP-Inflation]

Op basis van het automatisch schalen – schaal van regel voor het profiel van de weekdag van de productie serviceplan App:

![App Service plan voor werkdagen op basis van automatisch schalen – regel schaal van inflatie.][Equation1]

Bij het automatisch schalen – schaal van regel voor het Weekend-profiel van de productie serviceplan App, zou de formule omgezet in:

![App Service plan voor weekends op basis van automatisch schalen – regel schaal van inflatie.][Equation2]

Deze waarde kan ook worden berekend voor schaal-down-bewerkingen.

Op basis van het automatisch schalen – regel omlaag schaal voor het profiel van de weekdag van de productie serviceplan App, dit zou er als volgt uitzien:

![App Service plan voor werkdagen op basis van automatisch schalen – regel omlaag schaal inflatie.][Equation3]

Bij het automatisch schalen – regel omlaag schaal voor het Weekend-profiel van de productie serviceplan App, zou de formule omgezet in:  

![App Service plan voor weekends op basis van automatisch schalen – regel omlaag schaal inflatie.][Equation4]

De productie serviceplan App kan groeien op een maximale snelheid van acht exemplaren per uur in de week en vier exemplaren per uur tijdens het weekend. Deze kunt vrijgeven exemplaren met een maximale snelheid van vier exemplaren per uur in de week en zes exemplaren per uur tijdens het weekend.

Als meerdere App serviceplannen in een groep werknemers host, hebt u de *totale inflatie* wordt berekend als de som van de inflatie voor alle App Service plannen die worden in die werknemer van toepassingen die als host fungeert.

![De berekening van de totale inflatie voor meerdere App serviceplannen ondergebracht in een groep werknemers.][ASP-Total-Inflation]

### <a name="use-the-app-service-plan-inflation-rate-to-define-worker-pool-autoscale-rules"></a>De inflatie App Service plan gebruiken voor het definiëren van regels voor werknemer toepassingen automatisch schalen

Werknemer die host App serviceniveaus die zijn geconfigureerd om automatisch schalen moeten een buffer van de capaciteit worden toegewezen voor groepen. De buffer kan voor de bewerkingen automatisch schalen te vergroten of verkleinen van het abonnement App als nodig is. De minimale buffer worden de berekende totale App Service Plan inflatie.

Omdat de App omgeving schaal servicebewerkingen duren om toe te passen, moet elke wijziging rekening voor vraag verdere wijzigingen kunnen gebeuren tijdens een bewerking schalen. Het is raadzaam de berekende totale App Service Plan inflatie te gebruiken als het minimumaantal exemplaren die zijn toegevoegd voor elke bewerking automatisch schalen ten behoeve van deze latentie.

Met deze informatie kunt Frank het volgende automatisch schalen profiel en de regels definiëren:

![Regels voor automatisch schalen profiel bijvoorbeeld LOB.][Worker-Pool-Scale]

|   **Profiel automatisch schalen: weekdagen**                        |   **Profiel automatisch schalen: weekend**                |
|   ----------------------------------------------------    |   --------------------------------------------    |
|   **Naam:** Profiel van weekdag                               |   **Naam:** Weekend-profiel                       |
|   **Schalen door:** Planning- en regels            |   **Schalen door:** Planning- en regels    |
|   **Profiel:** Weekdagen                                   |   **Profiel:** Weekend                            |
|   **Type:** Terugkeerpatroon                                    |   **Type:** Terugkeerpatroon                            |
|   **Het doelbereik:** 13 tot en met 25 exemplaren                    |   **Doelbereik:** 6 tot en met 15 exemplaren             |
|   **Dagen:** Maandag, dinsdag, woensdag, donderdag, vrijdag  |   **Dagen:** Zaterdag, zondag                      |
|   **Begintijd:** 7:00 uur                                 |   **Begintijd:** 9:00 uur                         |
|   **Time zone:** UTC-08                                   |   **Time zone:** UTC-08                           |
|                                                           |                                                   |
|   **De regel automatisch schalen (schaal van)**                           |   **De regel automatisch schalen (schaal van)**                   |
|   **Bron:** Werknemer groep 1                             |   **Bron:** Werknemer groep 1                     |
|   **Metric:** WorkersAvailable                            |   **Metric:** WorkersAvailable                    |
|   **Bewerking:** Minder dan 8                              |   **Bewerking:** Minder dan 3                      |
|   **Duur:** 20 minuten                                |   **Duur:** 30 minuten                        |
|   **Tijd aggregatie:** Gemiddelde                           |   **Tijd aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verhogen door 8                         |   **Actie:** Aantal verhogen door 3                 |
|   **Afkoelen (minuten):** 180                            |   **Afkoelen (minuten):** 180                    |
|                                                           |                                                   |
|   **De regel automatisch schalen (schaal omlaag)**                         |   **De regel automatisch schalen (schaal omlaag)**                 |
|   **Bron:** Werknemer groep 1                             |   **Bron:** Werknemer groep 1                     |
|   **Metric:** WorkersAvailable                            |   **Metric:** WorkersAvailable                    |
|   **Bewerking:** Meer dan 8                           |   **Bewerking:** Groter dan 3                   |
|   **Duur:** 20 minuten                                |   **Duur:** 15 minuten                        |
|   **Tijd aggregatie:** Gemiddelde                           |   **Tijd aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verlagen door 2                         |   **Actie:** Aantal verlagen door 3                 |
|   **Afkoelen (minuten):** 120                            |   **Afkoelen (minuten):** 120                    |

Het doelbereik dat is gedefinieerd in het profiel wordt berekend door de minimale exemplaren die zijn gedefinieerd in het profiel voor de App serviceplan + buffer.

Het maximale bereik is de som van de maximale bereiken voor alle App serviceplannen ondergebracht in de groep van de werknemer.

Het aantal stijging voor de schaal van regels moet worden ingesteld op ten minste 1 X de inflatie App Service Plan voor schaal.

Daling aantal kan worden aangepast aan iets tussen X 1/2 of 1 X de inflatie App Service Plan voor schaal omlaag.

### <a name="autoscale-for-front-end-pool"></a>Automatisch schalen voor front-end toepassingen

Regels voor de front-automatisch schalen zijn voor werknemer toepassingen eenvoudiger. U moet in de eerste plaats,  
Zorg ervoor dat de duur van de meting en de timers cooldown kunt u bewerkingen op een App serviceplan schaal zijn niet onmiddellijk.

In dit scenario weet Frank dat de fout neemt toe nadat de front-ends 80% CPU-gebruik bereikt en wordt de regel automatisch schalen exemplaren als volgt verhogen:

![Instellingen voor automatisch schalen voor front-end toepassingen.][Front-End-Scale]

|   **Profiel automatisch schalen: voorkant eindigt**              |
|   --------------------------------------------    |
|   **Naam:** Automatisch schalen – voorkant eindigt                |
|   **Schalen door:** Planning- en regels    |
|   **Profiel:** Dagelijks gebruik                           |
|   **Type:** Terugkeerpatroon                            |
|   **Het doelbereik:** 3-10 exemplaren             |
|   **Dagen:** Dagelijks gebruik                              |
|   **Begintijd:** 9:00 uur                         |
|   **Time zone:** UTC-08                           |
|                                                   |
|   **De regel automatisch schalen (schaal van)**                   |
|   **Bron:** Front-end-toepassingen                    |
|   **Metric:** CPU-PERCENTAGE                               |
|   **Bewerking:** Meer dan 60%                 |
|   **Duur:** 20 minuten                        |
|   **Tijd aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verhogen door 3                 |
|   **Afkoelen (minuten):** 120                    |
|                                                   |
|   **De regel automatisch schalen (schaal omlaag)**                 |
|   **Bron:** Werknemer groep 1                     |
|   **Metric:** CPU-PERCENTAGE                               |
|   **Bewerking:** Minder dan 30%                    |
|   **Duur:** 20 minuten                        |
|   **Tijd aggregatie:** Gemiddelde                   |
|   **Actie:** Aantal verlagen door 3                 |
|   **Afkoelen (minuten):** 120                    |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png

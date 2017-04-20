<properties
 pageTitle="Preventief onderhoud vooraf geconfigureerde oplossing | Microsoft Azure"
 description="Een beschrijving van de Azure IoT preventief onderhoud vooraf geconfigureerde oplossing."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Preventief onderhoud vooraf geconfigureerde oplossing, overzicht

Het *preventief onderhoud* vooraf geconfigureerde oplossing is een van de [vooraf geconfigureerde oplossingen] [ lnk_preconfigured_solutions] uitgebracht als onderdeel van [Microsoft Azure IoT Suite][lnk_iot_suite]. Deze oplossing geïntegreerd apparaat real-time telemetrie-collectie met een voorspellende model gemaakt met [Azure Machine Learning][lnk_machine_learning].


Met Azure IoT Suite kunnen ondernemingen snel en eenvoudig verbinding maken met activa controleren en analyseren van gegevens in real-time. De voorspellende onderhoud vooraf geconfigureerde oplossing die gegevens nodig en bedrijven voorzien van nieuwe intelligentie die kan station efficiëntie en betere manieren om meer omzet met rijke dashboards en visualisaties.

## <a name="the-scenario"></a>Het Scenario

Fabrikam is een regionale luchtvaartmaatschappij dat zich op de ervaringen van uw klanten tegen scherpe prijzen richt. Een vlucht vertragingen veroorzaken onderhoudsproblemen en motor onderhoud van luchtvaartuigen is bijzonder moeilijk. Motor storing tijdens de vlucht moet vooral worden vermeden zodat Fabrikam zijn motoren regelmatig geïnspecteerd en aan een programma voor gepland onderhoud voldoet. Vliegtuigmotoren niet dezelfde echter altijd kunnen dragen. Sommige onnodig onderhoud wordt uitgevoerd op motoren. Belangrijker nog, problemen die kunnen een vliegtuig op de grond tot onderhoud wordt uitgevoerd. Hierdoor kostbare vertragingen, vooral als er een vliegtuig op een locatie waar de juiste technici of reserveonderdelen niet beschikbaar zijn.

De motoren van vliegtuigen van Fabrikam worden met sensoren die motor voorwaarden tijdens de vlucht controleren geïmplementeerd. Azure IoT Suite Fabrikam gebruiken voor het verzamelen van de sensorgegevens die zijn verzameld tijdens de vlucht. Van Fabrikam gegevens wetenschappers hebben een manier om te voorspellen van de resterende nuttige levensduur (RUL) van een vliegtuig motor gemodelleerd na opgebouwd jaren operationele engine en gegevens van de fout. Zij hebben geïdentificeerd is een relatie tussen de gegevens van vier van de sensoren engine met de slijtage van motor die tot een fout leidt. Terwijl Fabrikam blijft voor het uitvoeren van regelmatige inspecties om veiligheid te garanderen, kan deze de modellen nu gebruiken voor het berekenen van de RUL voor elke motor na elke vlucht met behulp van de telemetrie van motoren worden verzameld tijdens de vlucht. Fabrikam kan nu voorspellen van toekomstige punten storing en plan voor het onderhoud en herstellen van tevoren.

> [AZURE.NOTE] Gegevens voor werkelijke motor slijtage wordt gebruikt door het model van de oplossing.

Door het punt voorspellen wanneer onderhoud nodig is, Fabrikam kan hun activiteiten optimaliseren voor kosten te verlagen. Onderhoud coördinatoren werken met planners te plannen onderhoud samenviel met een vliegtuig op een bepaalde locatie stoppen en ervoor te zorgen dat voldoende tijd voor het vliegtuig niet meer service zonder verstoring van de planning op. Fabrikam kunt plannen technici ervoor zorgen dat luchtvaartuigen worden efficiënt onderhouden zonder wachttijd. Besturingselement voorraadbeheerders ontvangen onderhoudsplannen, zodat ze kunnen hun bestelproces optimaliseren en, onderdelenvoorraad reserveonderdelen. Dit alles kunnen Fabrikam vliegtuigen grond minimaliseren en om de bedrijfskosten en de veiligheid van passagiers en bemanning.

Om te begrijpen hoe [Azure IoT Suite] [ lnk_iot_suite] biedt mogelijkheden voor klanten moeten realiseren van het potentieel van preventief onderhoud, bekijk deze [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Hoe de oplossing voor preventief onderhoud wordt gemaakt

De oplossing maakt gebruik van een bestaand Azure Machine Learning model beschikbaar als de sjabloon weer te geven van deze mogelijkheden van apparaat telemetrie verzameld via IoT Suite-services werken. Microsoft heeft een [regressiemodel] ingebouwd[ lnk_regression_model] van de motor van een vliegtuig en de volledige sjabloon, gegevens gepubliceerd<sup>\[1\]</sup>, en stapsgewijze instructies over het gebruik van het model.

De IoT Azure preventief onderhoud vooraf geconfigureerde oplossing maakt gebruik van het regressiemodel gemaakt op basis van deze sjabloon. het is in uw abonnement Azure geïmplementeerd en toegankelijk zijn via een automatisch gegenereerde API. De oplossing omvat een subset van de testen gegevens over 4 (van in totaal 100) motoren en de 4 (21 totaal) sensor gegevensstromen die een nauwkeurig resultaat uit het model zijn opgeleid bieden.

*\[1\] A. Saxena en K. Goebel (2008). 'Turbofanmotoren Engine afbraak simulatie gegevensset', NASA Ames Prognostics gegevensopslagplaats (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett-veld, CA*

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over hoe Azure IoT scenario's voor preventief onderhoud kunnen, [waarde van de Internet zaken vastleggen][lnk_capture_value].

Een [scenario] te nemen[ lnk-predictive-walkthrough] vooraf geconfigureerde oplossing van het preventief onderhoud.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

U kunt ook een aantal andere functies en mogelijkheden van de IoT Suite vooraf geconfigureerde oplossingen verkennen:

- [Veelgestelde vragen voor IoT Suite][lnk-faq]
- [Beveiliging van de grond omhoog IoT][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

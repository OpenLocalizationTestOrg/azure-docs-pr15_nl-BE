<properties
 pageTitle="Preventief onderhoud procedure | Microsoft Azure"
 description="Een overzicht van de Azure IoT preventief onderhoud vooraf geconfigureerde oplossing."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
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

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Preventief onderhoud vooraf geconfigureerde oplossing scenario

## <a name="introduction"></a>Inleiding

De IoT Suite preventief onderhoud vooraf geconfigureerde oplossing is een end-to-end oplossing voor een zakelijke scenario dat het punt voorspelt als storing kunnen optreden. U kunt deze vooraf geconfigureerde oplossing proactief gebruiken voor activiteiten zoals het onderhoud te optimaliseren. De oplossing combineert belangrijke Azure IoT Suite-services, waaronder een [Azure Machine Learning] [ lnk_machine_learning] werkruimte. Deze werkruimte bevat experimenten, op basis van een openbare verzameling voorbeeldgegevens, te voorspellen van de resterende nuttige levensduur (RUL) van de motor van een vliegtuig. De oplossing implementeert het IoT scenario volledig als uitgangspunt voor u om te plannen en implementeren van een oplossing die voldoet aan uw eigen specifieke eisen.

## <a name="logical-architecture"></a>Logische architectuur

In het volgende diagram bevat een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![][img-architecture]

De blauwe artikelen zijn Azure services die worden ingericht op de locatie die u selecteert bij het inrichten van de vooraf geconfigureerde oplossing. U kunt de vooraf geconfigureerde oplossing in de regio van de Amerikaanse Oost-, Noord-Europa of Azië inrichten.

Sommige bronnen zijn niet beschikbaar in de regio's waar u de vooraf geconfigureerde oplossing inrichten. De oranje items in het diagram beschrijven de Azure services ingericht in de dichtstbijzijnde beschikbare regio (Zuid-centraal ons West-Europa en Zuidoost-Azië) gegeven van de geselecteerde regio.

Het groene item is een gesimuleerde apparaat met een motor van het vliegtuig. U kunt meer informatie over deze gesimuleerde apparaten in de volgende sectie.

De grijze items vertegenwoordigen componenten waarbij *apparaat beheer* mogelijkheden. De huidige versie van het preventief onderhoud vooraf geconfigureerde oplossing biedt deze bronnen niet inrichten. Voor meer informatie over het beheer van het apparaat, raadpleegt u de [externe controle vooraf geconfigureerde oplossing][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Gesimuleerde apparaten

Een gesimuleerde apparaat is in de vooraf geconfigureerde oplossing een vliegtuig motor. De oplossing is ingericht met twee motoren die worden toegewezen aan een enkele vliegtuigen. Elke engine genereert vier soorten telemetrie: 9 Sensor, Sensor 11 Sensor, 14 en 15 Sensor bevatten de gegevens die nodig zijn voor de Machine Learning model voor de berekening van de resterende nuttige levensduur (RUL) voor de motor. De volgende telemetrie berichten verzendt elke gesimuleerde apparaat naar IoT Hub:

*Aantal maal*. Een cyclus geeft een voltooide vlucht van variabele lengte tussen 2-10 uur waarin telemetrie data elk half uur tijdens de vlucht wordt vastgelegd.

*Telemetrie*. Er zijn vier sensoren die vertegenwoordigen de kenmerken van de motor. De sensoren zijn generieke label Sensor 9, 11 Sensor, Sensor, 14 en 15 Sensor. Deze 4 sensoren vertegenwoordigen telemetrie voldoende bruikbare resultaten ophalen uit het model van de Machine Learning voor RUL. Dit model is gemaakt van een openbare gegevensverzameling die echte engine sensorgegevens bevat. Zie voor meer informatie over hoe het model van de oorspronkelijke gegevensset is gemaakt, de [Cortana Intelligence galerie preventief onderhoud sjabloon][lnk-cortana-analytics].

De gesimuleerde apparaten kunnen de volgende opdrachten vanaf een hub IoT verzonden worden verwerkt:

| Opdracht | Beschrijving |
|---------|-------------|
| StartTelemetry | Hiermee bepaalt de status van de simulatie.<br/>Start het apparaat verzenden telemetrie     |
| StopTelemetry  | Hiermee bepaalt de status van de simulatie.<br/>Hiermee stopt u het apparaat verzenden telemetrie |

IoT Hub biedt apparaat opdracht bevestiging.

## <a name="azure-stream-analytics-job"></a>Azure Analytics Stream-project

**Project: telemetrie** werkt op de binnenkomende apparaat telemetrie stroom met behulp van twee instructies. De eerste alle telemetrie selecteert in apparaten en stuurt deze gegevens naar een blob-opslag van waar deze wordt weergegeven in de web app. De tweede instructie berekent sensor gemiddelde waarden over een schuifvenster bij twee minuten en verzendt deze gegevens via de hub gebeurtenis op een **gebeurtenis processor**.

## <a name="event-processor"></a>Gebeurtenisverwerking

De **gebeurtenis processor** heeft de sensor gemiddelde waarden voor een voltooide cyclus. Deze de gangen die waarden naar een API die beschikbaar worden gesteld van de Machine Learning opgeleid model voor het berekenen van de RUL voor een motor.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Zie voor meer informatie over hoe het model van de oorspronkelijke gegevensset is gemaakt, de [Cortana Intelligence galerie preventief onderhoud sjabloon][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Laten we beginnen lopen

Deze sectie helpt u bij de onderdelen van de oplossing, beschrijft het geval van het beoogde gebruik, en voorbeelden.

### <a name="predictive-maintenance-dashboard"></a>Dashboard voor preventief onderhoud

Deze pagina in de webtoepassing gebruikt PowerBI JavaScript-besturingselementen (Zie de [opslagplaats PowerBI visuele][lnk-powerbi]) om te visualiseren:

- De uitvoergegevens van de Stream Analytics-projecten in de blob-opslag.
- Het aantal RUL en cyclus per vliegtuig-engine.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Inachtneming van het gedrag van de cloud-oplossing

Ga naar de bronnengroep met de naam van de oplossing die u hebt gekozen om uw ingerichte resources weer te geven in het portal voor Azure.

![][img-resource-group]

Wanneer u de vooraf geconfigureerde oplossing inrichten, ontvangt u een e-mailbericht met een koppeling naar de werkruimte Machine Learning. U kunt ook navigeren naar de werkruimte Machine Learning uit de [azureiotsuite.com] [ lnk-azureiotsuite] pagina voor uw ingerichte oplossing wanneer deze in de toestand **Gereed** is.

![][img-machine-learning]

U kunt zien dat het monster is ingericht met vier gesimuleerde apparaten voor twee vliegtuigen met twee motoren per vliegtuig, elk met vier sensoren in de portal oplossing. Als u eerst naar de portal oplossing gaat, wordt de simulatie gestopt.

![][img-simulation-stopped]

Klik op **Start simulatie** om te beginnen met de simulatie waarin u de geschiedenis van de sensor, RUL, cycli, Zie en RUL geschiedenis vullen het dashboard.

![][img-simulation-running]

RUL is minder dan 160 (een willekeurige drempel voor demonstratiedoeleinden gekozen), de portal oplossing een waarschuwingssymbool naast de RUL weergave wordt weergegeven als de motor van het vliegtuig in geel gemarkeerd. U ziet hoe de RUL waarden hebben een algehele algemene neerwaartse trend, maar veelal Stuiteren omhoog en omlaag. Dit probleem wordt veroorzaakt door de verschillende lengtes van de cyclus en de nauwkeurigheid van het model.

![][img-simulation-warning]

De volledige simulatie duurt ongeveer 35 minuten 148 cycli. De drempel van 160 RUL is voldaan voor de eerste keer op ongeveer 5 minuten en beide motoren de drempel bereikt op ongeveer 8 minuten.

De simulatie wordt uitgevoerd door middel van de volledige gegevensset voor 148 cycli en vereffent op resultaatwaarden van RUL en cyclus.

U kunt de simulatie op elk gewenst moment stoppen, maar de simulatie van het begin van de dataset te klikken op **Start simulatie** terugspelen.

## <a name="next-steps"></a>Volgende stappen

Nu u de preventief onderhoud vooraf geconfigureerde oplossing die u wilt wijzigen, raadpleegt u [richtlijnen voor het aanpassen van vooraf geconfigureerde oplossingen]hebt uitgevoerd[lnk-customize].

Het weblogbericht [IoT Suite - onder de kap - preventief onderhoud](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet biedt extra details over het preventief onderhoud vooraf geconfigureerde oplossing.

U kunt ook een aantal andere functies en mogelijkheden van de IoT Suite vooraf geconfigureerde oplossingen verkennen:

- [Veelgestelde vragen voor IoT Suite][lnk-faq]
- [Beveiliging van de grond omhoog IoT][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md

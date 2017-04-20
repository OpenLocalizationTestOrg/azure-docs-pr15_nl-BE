<properties
 pageTitle="Overzicht van IoT Hub device management | Microsoft Azure"
 description="Dit artikel biedt een overzicht van Apparaatbeheer in Azure IoT Hub: enterprise apparaat lifecycle, opnieuw opstarten, factory reset, firmware-update, configuratie, apparaat twins, query's, taken"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Overzicht van Apparaatbeheer Azure IoT Hub (voorbeeld)

## <a name="introduction"></a>Inleiding

Azure IoT Hub biedt de functies en een uitbreidbaarheidsmodel die het apparaat en de back-end ontwikkelaars krachtige oplossingen voor IoT apparaat beheer. IoT apparaten bereik van beperkte sensoren en één doel microcontrollers, krachtige gateways die communicatie voor groepen van apparaten route.  Bovendien verschillen de use-cases en eisen voor de marktdeelnemers IoT aanzienlijk tussen industrieën.  Ondanks deze variatie zorgt Azure IoT Hub Apparaatbeheer voor de mogelijkheden, patronen en codebibliotheken geschikt zijn voor een groot aantal verschillende apparaten en eindgebruikers.

Een cruciaal onderdeel van het maken van een succesvolle onderneming IoT oplossing te voorzien in een strategie hoe operators omgaan met het dagelijks beheer van de collectie van apparaten. IoT operators vereisen eenvoudige en betrouwbare hulpprogramma's en toepassingen waarmee ze zich concentreren op de strategische aspecten van hun taken. Dit artikel bevat:

- Een kort overzicht van Apparaatbeheer Azure IoT Hub aanpak.
- Een beschrijving van de gemeenschappelijke beginselen van de apparaat beheer.
- Een beschrijving van de levenscyclus van het apparaat.
- Een overzicht van algemene device management patronen.

## <a name="iot-device-management-principles"></a>Beginselen van IoT apparaat beheer

IoT brengt daarmee een unieke set device management uitdagingen en bieden elke oplossing voor enterprise-klasse de volgende beginselen:

![Azure IoT Hub device management principes afbeelding][img-dm_principles]

- **Schaal en automatisering**: IoT oplossingen vereisen eenvoudige hulpmiddelen die kunnen routinetaken automatiseren en een relatief kleine beheerders voor het beheren van miljoenen apparaten inschakelen. Dagelijkse, operatoren verwachten bewerkingen apparaat op afstand, in bulk, en alleen worden gewaarschuwd wanneer er zich problemen voordoen die hun directe aandacht vereisen.

- **Openheid en compatibiliteit**: de IoT apparaatecosysteem is buitengewoon Divers. Beheerprogramma's moeten worden aangepast ten behoeve van een groot aantal apparaatklassen, platforms en protocollen. Operators moeten kunnen bieden vele soorten apparaten uit de meest beperkte ingesloten één proces-chips, computers met krachtige en volledig functioneel zijn.

- **Context awareness**: IoT omgevingen zijn dynamische, steeds veranderende. Betrouwbaarheid van de service is uitermate belangrijk. Apparaat beheerbewerkingen moeten factor SLA windows onderhoud, netwerk- en lidstaten voorwaarden in gebruik en geolocation apparaat om ervoor te zorgen dat downtime onderhoud niet kritieke bedrijfsactiviteiten van invloed zijn op of gevaarlijke omstandigheden te maken.

- **Veel rollen service**: ondersteuning voor de unieke workflows en processen IoT van functies van operations is van cruciaal belang. Het operationele personeel moet eenheid in de omschrijving werken met de gegeven beperkingen van interne IT-afdelingen.  Duurzame manieren om realtime oppervlak apparaatgegevens bewerkingen toezichthouders en andere zakelijke leidinggevende rollen moeten ze ook vinden.

## <a name="iot-device-lifecycle"></a>IoT apparaat lifecycle

Er is een reeks algemene device management stadia die gemeenschappelijk voor alle ondernemingsprojecten IoT zijn. In Azure IoT zijn er vijf fasen binnen de levenscyclus IoT apparaat:

![De vijf fasen van Azure IoT apparaat levenscyclus: plannen, inrichten, configureren, bewaken, intrekken][img-device_lifecycle]

Binnen elk van deze vijf fasen zijn er verschillende apparaat operator vereisten moeten worden voldaan om een volledige oplossing:

- **Plan**: operators voor het maken van een apparaat metagegevens schema kunnen ze gemakkelijk en nauwkeurig zoeken en een groep van hulpmiddelen voor beheer van bulkbewerkingen inschakelen. U kunt het apparaat twin dit Apparaatmetagegevens worden opgeslagen in de vorm van labels en eigenschappen.

    *Verder lezen*: [aan de slag met apparaat twins][lnk-twins-getstarted], [Understand apparaat twins][lnk-twins-devguide], [twin-eigenschappen gebruiken][lnk-twin-properties]

- **Voorziening**: veilig inrichten van nieuwe apparaten op de IoT Hub en inschakelen van operatoren onmiddellijk apparaat mogelijkheden te ontdekken.  Het register IoT Hub apparaat met flexibele apparaat-id's en referenties te maken en deze bewerking uitvoeren in grote hoeveelheden met behulp van een taak. Apparaten om de mogelijkheden en voorwaarden via de eigenschappen van een apparaat in het apparaat twin rapport samenstellen.

    *Verder lezen*: [apparaat-identiteiten beheren][lnk-identity-registry], [Bulk beheer van identiteiten apparaat][lnk-bulk-identity], [twin-eigenschappen gebruiken][lnk-twin-properties]

- **Configureren**: vergemakkelijken configuratiewijzigingen van bulk- en firmware-updates voor apparaten behoud van gezondheids- en beveiliging. Deze device management bewerkingen uitvoeren in bulk met de gewenste eigenschappen of met de rechtstreekse methoden en broadcast taken.

    *Verder lezen*: [directe methoden][lnk-c2d-methods], [Invoke een directe methode op een apparaat][lnk-methods-devguide], [twin-eigenschappen gebruiken][lnk-twin-properties], [planning en uitzending taken][lnk-jobs], [taken plannen op meerdere apparaten][lnk-jobs-devguide]

- **Monitor**: algemene apparaat collectie toestand, de status van de dagelijkse bedrijfsvoering bewaken en waarschuwt operators voor problemen die misschien hun aandacht nodig hebben.  Het apparaat twin als apparaten rapport realtime bedrijfsomstandigheden en status van de update van toepassing. Krachtige dashboardrapporten die de meest directe problemen over het oppervlak met behulp van apparaat twee query's maken.

    *Verder lezen*: [het gebruik van eigenschappen twin][lnk-twin-properties], [querytaal voor twins en taken][lnk-query-language]

- **Intrekken**: vervangen of apparaten na een storing te ontmantelen, upgrade cyclus, of aan het einde van de levensduur van de service.  Gebruik het apparaat twin apparaatgegevens onderhouden als het fysieke apparaat wordt vervangen of gearchiveerd als wordt ingetrokken. Het register IoT Hub apparaat gebruiken voor het intrekken van veilig apparaat-id's en referenties.

    *Verder lezen*: [het gebruik van eigenschappen twin][lnk-twin-properties], [apparaat-identiteiten beheren][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT Hub device management patronen

IoT Hub, kunnen de volgende set device management patronen.  De [zelfstudies voor device management] [ lnk-get-started] weergeven in meer detail het uitbreiden van deze patronen aanpassen aan uw exacte scenario en het ontwerpen van nieuwe patronen op basis van deze core-sjablonen.

- **Opnieuw opstarten** - back-endtoepassing meldt aan het apparaat via een directe methode deze opnieuw tot stand heeft gebracht.  Het apparaat wordt het apparaat gebruikt twin eigenschappen voor het bijwerken van de status van het opnieuw opstarten van het apparaat gemeld.

    ![Azure IoT Hub device management patroon afbeelding opnieuw opstarten][img-reboot_pattern]

- **Factory Reset** - back-endtoepassing meldt aan het apparaat via een directe methode dat er een factory reset heeft gestart.  Het apparaat maakt gebruik van het apparaat twin gerapporteerde eigenschappen voor het bijwerken van de fabriek status van het apparaat opnieuw ingesteld.

    ![Azure IoT Hub device management fabriek herstellen patroon afbeelding][img-facreset_pattern]

- **Configuratie** - back-endtoepassing worden de eigenschappen van de gewenste twin apparaat software die wordt uitgevoerd op het apparaat te configureren.  Het apparaat wordt het apparaat gebruikt twin eigenschappen voor het bijwerken van de configuratiestatus van het apparaat gemeld.

    ![Azure IoT Hub device management configuratie patroon afbeelding][img-config_pattern]

- **Firmware-Update** - back-endtoepassing meldt aan het apparaat via een directe methode dat er een firmware-update heeft gestart.  Het apparaat start een dergelijk complex proces om de firmware te downloaden, het pakket firmware en ten slotte de IoT Hub-service opnieuw.  Gedurende het hele mult-stap proces, het apparaat wordt het apparaat gebruikt twin eigenschappen voor het bijwerken van de voortgang en status van het apparaat gemeld.

    ![Patroon-afbeelding van Azure IoT Hub device management-firmware bijwerken][img-fwupdate_pattern]

- **Rapportage van de voortgang en status** - de toepassing van de back-end apparaat twee query's uitgevoerd voor een reeks apparaten om te rapporteren over de status en voortgang van acties die worden uitgevoerd op de apparaten.

    ![Azure IoT Hub Apparaatbeheer voortgang en status patroon afbeelding melden][img-report_progress_pattern]

## <a name="next-steps"></a>Volgende stappen

U kunt de mogelijkheden, patronen en codebibliotheken die Azure IoT Hub Apparaatbeheer biedt, om IoT toepassingen maken die voldoen aan de eisen van enterprise IoT operator in in elke fase van de levenscyclus van apparaat.

Zie de [aan de slag met Azure IoT Hub Apparaatbeheer] om door te gaan met meer informatie over functies voor het apparaat Azure IoT Hub[ lnk-get-started] zelfstudie.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md
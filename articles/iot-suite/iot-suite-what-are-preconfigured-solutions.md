<properties
 pageTitle="Azure IoT vooraf geconfigureerde oplossingen | Microsoft Azure"
 description="Een beschrijving van de IoT Azure vooraf geconfigureerde oplossingen en hun architectuur met koppelingen naar aanvullende bronnen."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Wat zijn de Azure IoT Suite vooraf geconfigureerde oplossingen?

De Azure IoT Suite vooraf geconfigureerde oplossingen zijn implementaties van algemene IoT oplossing patronen die u kunt gebruiken voor uw abonnement met Azure. U kunt de vooraf geconfigureerde oplossingen gebruiken:

- Als uitgangspunt voor uw eigen IoT oplossingen.
- Voor meer informatie over algemene patronen in IoT oplossing ontwerpen en ontwikkelen.

Elke vooraf geconfigureerde oplossing is een complete, end-to-end implementatie die gesimuleerde apparaten worden gebruikt voor het genereren van telemetrie.

Naast het implementeren en uitvoeren van de oplossingen in Azure, kunt u downloaden van de volledige broncode en vervolgens aanpassen en uitbreiden van de oplossing om te voldoen aan uw specifieke vereisten voor IoT.

> [AZURE.NOTE] Als u wilt een van de vooraf geconfigureerde oplossingen implementeren, gaat u naar [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Het artikel [aan de slag met de vooraf geconfigureerde IoT oplossingen] [ lnk-getstarted-preconfigured] vindt u meer informatie over het implementeren en uitvoeren van een van de oplossingen.

De volgende tabel ziet u hoe de oplossingen die worden toegewezen aan specifieke IoT-functies:

| Oplossing | Opname van de gegevens | Apparaat-id | Opdrachten en besturing | Regels en acties | Voorspellende Analytics |
|------------------------|-----|-----|-----|-----|-----|
| [Externe controle][lnk-getstarted-preconfigured] | Ja | Ja | Ja | Ja | -   |
| [Preventief onderhoud][lnk-predictive-maintenance] | Ja | Ja | Ja | Ja | Ja |

- *Opname van de gegevens*: Ingress bij schalen naar de cloud.
- *Apparaat-id*: unieke identiteiten voor elk aangesloten apparaat te beheren.
- *Opdrachten en besturing*: berichten verzenden naar een apparaat uit de cloud aan dat het apparaat iets moet doen.
- *Regels en acties*: de oplossing voor back-end regels gebruikt om te reageren op specifieke apparaat-wolk-gegevens.
- *Voorspellende analytics*: de oplossing voor back-end van toepassing analyseert apparaat-wolk-gegevens om te voorspellen wanneer bepaalde acties moeten plaatsvinden. Analyseren bijvoorbeeld vliegtuigen engine telemetrie om te bepalen wanneer de motor onderhoud worden betaald.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Externe controle vooraf geconfigureerde oplossing-overzicht

We hebt gekozen voor de externe controle vooraf geconfigureerde oplossing in dit artikel bespreken omdat ziet u veel algemene ontwerpelementen die delen van de andere oplossingen.

In het volgende diagram ziet u de belangrijkste elementen van de externe monitoring oplossing. In de volgende secties bevatten meer informatie over deze elementen.

![Externe controle vooraf geconfigureerde oplossingsarchitectuur][img-remote-monitoring-arch]

## <a name="devices"></a>Apparaten

Wanneer u de externe controle vooraf geconfigureerde oplossing implementeert, zijn vier gesimuleerde apparaten vooraf ingerichte in de oplossing die een koeling apparaat simuleren. Deze gesimuleerde apparaten hebben een ingebouwde temperatuur en vochtigheid model telemetrie genereert. Deze gesimuleerde apparaten zijn opgenomen ter illustratie van de end-to-end-stroom van gegevens door de oplossing en het bieden van een handige bron van Telemetrie en een doel voor opdrachten als u een back-end ontwikkelaar met behulp van de oplossing als uitgangspunt voor een aangepaste toepassing.

Wanneer een apparaat eerst verbinding maakt met IoT Hub in de externe controle vooraf geconfigureerde oplossing, het apparaat informatiebericht verzonden naar de hub IoT servernetwerkprotocollen wordt de lijst met opdrachten die het apparaat kan reageren op. De opdrachten zijn in de externe controle vooraf geconfigureerde oplossing: 

- *Ping-apparaat*: het apparaat reageert op deze opdracht met een bevestiging. Dit is handig voor het controleren van het apparaat is nog steeds actief en luistert.
- *Telemetrie start*: Hiermee geeft u het apparaat om te beginnen met het verzenden van telemetrie.
- *Telemetrie stoppen*: Hiermee geeft u het apparaat om te stoppen met het verzenden van telemetrie.
- *Wijziging Set punt temperatuur*: regelt de temperatuur gesimuleerde telemetrie waarden het apparaat verzonden. Dit is handig voor het testen van back-end-logica.
- *Diagnostische telemetrie*: als het apparaat moet de externe temperatuur worden verzonden als telemetrie bepaalt.
- *Status van het apparaat wijzigen*.: de eigenschap apparaat staat metagegevens die het apparaat gemeld wordt. Dit is handig voor het testen van back-end-logica.

U kunt meer gesimuleerde apparaten toevoegen aan de oplossing die de dezelfde telemetrie uitstralen en reageren op dezelfde opdrachten. 

## <a name="iot-hub"></a>IoT Hub

In deze vooraf geconfigureerde oplossing het IoT Hub exemplaar komt overeen met de *Cloud Gateway* in een typische [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

Een hub IoT ontvangt telemetrie van de apparaten op een enkel eindpunt. Een hub IoT onderhoudt ook een apparaat-specifieke eindpunten waar de opdrachten die worden verzonden naar het door elke apparaten kunnen ophalen.

De hub IoT beschikbaar de telemetrie ontvangen via de service-side telemetrie eindpunt te lezen.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

De vooraf geconfigureerde oplossing maakt gebruik van drie [Azure Stream Analytics] [ lnk-asa] (ASA)-taken voor het filteren van de stroom van de telemetrie van de apparaten:


- *Taak DeviceInfo* - voert de gegevens op de hub van een gebeurtenis die routes inschrijving bepaalde berichten, verzonden als een apparaat voor het eerst verbinding maakt, of in antwoord op een **status van het apparaat wijzigen** , opdracht in het register van oplossing apparaat (een DocumentDB-database). 
- *Telemetrie-taak* - alle ruwe telemetrie verzendt naar Azure blob-opslag voor gekoelde opslag en telemetrie aggregaties die worden weergegeven in het dashboard oplossing berekend.
- *Regels voor taak* - filters stream telemetrie voor waarden die groter zijn dan de drempelwaarden voor de regel en voert de gegevens op een hub gebeurtenis. Wanneer een regel wordt gestart, wordt de weergave van de portal dashboard oplossing wordt deze gebeurtenis als een nieuwe rij in de tabel alarm geschiedenis en een actie op basis van de instellingen die zijn gedefinieerd in de regels en acties in de portal oplossing activeert.

In deze vooraf geconfigureerde oplossing de ASA taken deel uitmaken van de **back-end IoT-oplossing** in een typische [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Gebeurtenisverwerking

In deze vooraf geconfigureerde oplossing de gebeurtenisverwerking deel uitmaakt van de **back-end IoT-oplossing** in een typische [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

De **DeviceInfo** en **regels** ASA taken verzenden hun uitvoer naar gebeurtenis hubs voor levering aan een andere back-end services. De oplossing maakt gebruik van een [EventPocessorHost] [ lnk-event-processor] exemplaar, dat wordt uitgevoerd in een [WebJob][lnk-web-job], de berichten lezen van deze gebeurtenis hubs. De **EventProcessorHost** **DeviceInfo** gegevens gebruikt voor het bijwerken van de apparaatgegevens in de DocumentDB database en de gegevens **regels** worden gebruikt om op te roepen de logica app en update de waarschuwingen worden weergegeven in de portal oplossing.

## <a name="device-identity-registry-and-documentdb"></a>Register van apparaat-id en DocumentDB

Elke hub IoT bevat een [apparaat identiteit register] [ lnk-identity-registry] die apparaat sleutels worden opgeslagen. IoT Hub gebruikt deze informatie apparaten verifiëren - een apparaat moet worden geregistreerd en hebben een geldige sleutel voordat kan worden aangesloten op de hub.

Deze oplossing bevat meer informatie over apparaten zoals de staat en de opdrachten die ze ondersteunen, andere metagegevens. De oplossing maakt gebruik van een DocumentDB database voor het opslaan van deze apparaatgegevens voor de oplossing specifieke en de portal oplossing haalt gegevens op uit deze database DocumentDB voor het weergeven en bewerken.

De oplossing moet ook de informatie in het apparaat identiteit register gesynchroniseerd met de inhoud van de database DocumentDB behouden. De **EventProcessorHost** gebruikt de gegevens uit **DeviceInfo** stream analytics taak beheert u de synchronisatie.

## <a name="solution-portal"></a>Oplossing-portal

![Dashboard oplossing][img-dashboard]

Het portaal oplossing is een web gebaseerde gebruikersinterface die naar de cloud is geïmplementeerd als onderdeel van de vooraf geconfigureerde oplossing. U kunt:

- Telemetrie- en alarmsystemen geschiedenis weergeven in een dashboard.
- Inrichten van nieuwe apparaten.
- Apparaten controleren en beheren.
- Opdrachten verzenden naar specifieke apparaten.
- Regels en acties beheren.

In deze vooraf geconfigureerde oplossing het portaal oplossing vormt onderdeel van de **back-end oplossing IoT** en onderdeel van de **verwerking en business connectivity** in de typische [oplossingsarchitectuur IoT][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over IoT oplossing architecturen, [Microsoft Azure IoT services: referentiearchitectuur][lnk-refarch].

Nu u welke vooraf geconfigureerde oplossing weet is, u kunt aan de slag met de implementatie van de *externe controle* vooraf geconfigureerde oplossing: [aan de slag met de vooraf geconfigureerde oplossingen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md